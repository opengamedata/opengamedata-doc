# Adding a Feature

In order to add a new feature for a game, you must complete the following steps:

## **Code a feature extractor**

In this step, you need to write the individual feature extractor.
This should be a python file, placed in the `<ogd-core-root>/games/<GAME_NAME>/extractors` folder, which inherits from the Feature class.
At minimum, you must create an `__init__(...)` function, and implement the `GetEventTypes(self)`, `CalculateFinalValues(self)`, and `_extractFromEvent(self, event)` functions.  
You are free to add whatever `__init__` parameters you like, but you should at minimum include parameters to map to `name`, `description`, and `count_index` when calling the superclass constructor.
Do not change the parameters of the other functions, as they are implementing abstract functions of the base class.  

- `GetEventTypes(self) -> List[str]` should simply return a list of the names of the events your Feature would like to analyze.  
  *For example*, a feature to calculate the time spent on a level should request the game's "start level" and "end level" events.  
- `_extractFromEvent(self, event:Event)` will run once on each occurrence of an event whose type is requested in `GetEventTypes`.
  This function is where you will tabulate whatever data you need from individual events.  
  *For example*, a feature to calculate the number of times a player clicked a button would create a "count" variable in its `__init__` function, and increment that variable here.
- `GetFeatureValues(self) -> List[Any]` should return a list containing whatever the "metric" or *value* of the Feature is, given whatever events have been seen so far.  
  *For example*, a feature to calculate the average number of moves a player made per level would:  
  1. add up all moves (as recorded with `_extractFromEvent`)  
  2. count the number of levels started (as recorded with `_extractFromEvent`)  
  3. divide the move count by the number of levels.  
  4. return a list containing the calculated average.  

  For a basic feature, there should only be one object in the returned list, although that one object could contain many elements.
  For cases where you place multiple objects into the returned list, see the "**Subfeatures**" section under "**Optional Feature features**."  

## **Add feature to extractors package**

Next, you need to ensure your new feature is included when the top-level Extractor class imports the `extractors` folder.
Open the `<ogd-core-root>/games/<GAME_NAME>/extractors/__init__.py` file, which should look something like this:

    ```python
    __all__ = [
        "AverageLevelTime",
        "LevelCompletionTime",
        "SessionDuration",
        "SessionID",
    ]

    from . import AverageLevelTime
    from . import LevelCompletionTime
    from . import SessionDuration
    from . import SessionID

    ```

    Add the name of your feature class to the `__all__` list, and add the `import` for your feature class in the list of imports below.

## **Register feature in the top-level Extractor**

Now, we must ensure your game's Extractor class knows how to load actual instances of your Feature.
This will be done in the `<ogd-core-root>/games/<GAME_NAME>/<GameName>Extractor.py` file
(as an aside, note that the convention is to name the `<GAME_NAME>` folder with ALL CAPS, while the naming convention for Extractor classes is to use PascalCase).
Open up the `<GameName>Extractor.py` file, and look for the `_loadFeature` function, whose body should look something like this:

    ```python
    def _loadFeature(self, feature_type:str, name:str, feature_args:Dict[str,Any], count_index:Union[int,None] = None) -> Feature:
        ret_val : Feature
        if feature_type == "AverageLevelTime":
            ret_val = AverageLevelTime.AverageLevelTime(name, feature_args["description"])
        elif feature_type == "LevelCompletionTime":
            ret_val = LevelCompletionTime.LevelCompletionTime(name, feature_args["description"], count_index)
        elif feature_type == "SessionDuration":
            ret_val = SessionDuration.SessionDuration(name, feature_args["description"])
        elif feature_type == "SessionID":
            ret_val = SessionID.SessionID(name, feature_args["description"], self._session_id)
        else:
            raise NotImplementedError(f"'{feature_type}' is not a valid feature for Aqualab.")
        return ret_val

    ```

    Add a case for your new Feature class, checking if `feature_type` matches the name of your Feature class (more specifically, you're checking if it matches the name you give your feature in the configuration file in step 4, but using the name of the Feature class is a *strongly recommended* convention).
    Within this case, set the `ret_val` to a new instance of your Feature, passing in whatever parameters you added to your `__init__` function.
    Note that any parameters you add to `__init__` must be for data the top-level Extractor class can access and pass to the constructor.

## **Add feature configuration to the game's schema**

All of the coding required was covered in the first three steps.
You wrote the code to perform extraction, then registered the class in the set of extractors, and added code to create instances of your Feature extractor.
The final step is to adjust the configuration for the game, so that the system will include the Feature when performing data exports.
Here, you will need to open `<ogd-core-root>/games/<GAME_NAME>/<GAME_NAME>.json`.
This file will have the following layout:

```json
{
    "level_range": { 
        ...
    },

    "events": {
        ...
    },

    "features": {
        "per_count": {
            "LevelCompletionTime": {
                "enabled": false,
                "count":"level_range",
                "prefix": "job",
                "description": "Time taken to complete a given level"
            },
        },
        "aggregate":{
            "AverageLevelTime": {
                "enabled": false,
                "description":"Average time spent per level played"
            },
            "SessionDuration": {
                "enabled": true,
                "description":"Time spent playing in a given session"
            },
            "SessionID": {
                "enabled": true,
                "description":"The player's session ID number for this play session"
            }
        }
    },

    "db_columns": {
        ...
    },
    "config": {
        ...
    },
}

```

Note that the ellipses are stand-ins for the actual content found in the file - those contents are not important for this step.  
You will add a configuration for your feature in the features dictionary.
There are two fundamental kinds of features:  

- "aggregate" features, which will have one instance per gameplay session (e.g. the overall duration of a session).
These are also called "session" features.  
- "per-count" features, which will have a defined number of instances for each session (e.g. the time spent individually on each level, or responses to each of four pre-game survey questions).

If your Feature is meant to function as an "aggregate" feature, you will add it to the `aggregate` dictionary.
Each item maps the name of the Feature to a sub-dictionary.
We **strongly** recommend using the name of your `Feature` subclass (as created in step 1) as the name here, although technically you may use any name you want.  
The sub-dictionary for your Feature needs two elements: "enabled" and "description," where "enabled" is a boolean telling the system whether to use this Feature, and "description" is a human-readable description of what the Feature calculates.
If desired, you can add extra elements to the sub-dictionary for use as parameters of your Feature.
For example, if you would like to parameterize a feature to use either the mean or median as a measure of center, you could add the following to your `aggregate` dictionary:

```json
  "TypicalLevelTime" : {
      "enabled" : true,
      "center" : "median",
      "description" : "Calculates either the player's average or median level time, depending on the parameter"
  }
```

You would then have access to the parameter in step 3 (adding the feature to `_loadFeature`) as an element of `feature_args`.

On the other hand, if your Feature is meant to function as a "per-count" feature, you'll add it to the `per_count` dictionary.
The process is similar to adding a feature to `aggregate`, requiring "enabled" and "description" as elements.
In the "per-count" case, you will need to include "count" and "prefix" elements as well.

- "count" is an integer telling the system how many instances of the Feature to create for each gameplay session.  
*For example*, if your Feature records responses to each of four survey questions in a game, you'll want to set count to 4.
Then each session will get four instances of the feature, numbered 0, 1, 2, and 3.  
As a convenience, you may set count to the string "level_range", and the system will use the range of values defined in "level_range" within the `<GAME_NAME>.json` file.
This is not a general feature, and any other strings will fail.
The feature only exists because many games have a concept of level, and levels are often a useful unit of analysis.
When in doubt, default to using an integer value for the count.
- "prefix" maps to a string, which will be used to help distinguish different classes of per-count feature.  
*For example*, suppose you have ClickCount as a feature counting number of clicks on each level, and Response as a feature recording survy responses.
You might choose "level" as a prefix for ClickCount, and "survey" as a prefix for Response.
Then the final feature names for each of those Feature instances will be:  
```level0_ClickCount, level1_ClickCount, level2_ClickCount, ... , survey0_Response, survey1_Response, ...```  
When browsing columns in a spreadsheet of Feature values, it becomes much easier to distinguish which features occur per-level, which occur per-survey-item, etc.

As with "aggregate" features, "per-count" features can have additional elements in their sub-dictionaries, which can then be accessed for use as parameters in the `feature_args` variable in `_loadFeature`.

## Optional Feature features

The preceding sections discussed the minimum requirements for creating a Feature subclass.
However, there are additional features (lower-case 'f', meaning *features* as in "things a system can do") available to you when programming your Feature:  

### **Feature sub-types**

There are a few "sub-types" of the Feature class, which add provide small additional conveniences. Your Feature subclass can inherit from these types instead of the base `Feature`:  

- `PerLevelFeature`  
This Feature subclass adds validation to automatically check if the `'level'` element of each incoming Event object matches the current Feature object's `_count_index` element, and ignores the Event if it does not.  
This ensures the `_extractFromEvent` function is only run on the instance of a given Feature intended to process data for that level.

### **Subfeatures**  

A single Feature class can be written as a composite of multiple metrics, with each metric having its own column in the output.
Each additional metric is considered a "Subfeature" of the Feature subclass.  
*for example*, a Feature class called `LevelComplete` could calculate:  

- a boolean metric, indicating whether a given level was completed
- a "completion count" metric, indicating the number of times the level was completed  

To do this, you will need to carry out the following additional steps when writing your Feature:

1. In the "**Code a feature extractor**" step, you must implement another function:  
  - `Subfeatures(self) -> List[str]` should return a list of names for each subfeature (additional metric) the Feature class provides.  
  Your output will contain one column with the "base" Feature value, and one additional column for each "subfeature" value.  
  *For example*: In the `LevelComplete` example, the `Subfeatures` function may return `['Count']`.
  Then in the output, there will be one column named "LevelComplete", and another named "LevelCompleteCount."  
2. In the "**Code a feature extractor**" step, you will add an object to the list returned from `GetFeatureValues()` for each subfeature.  
The first item in the list will go in the output column with the "base" Feature name, the next in the list will go in the column with the first "subfeature" name, etc.  
*For example*, continuing the `LevelComplete` example:
  - the first object in the list may be the `bool` value indicating whether the given level was completed, and goes to the `LevelComplete` column.  
  - the second object may be the "count" value indicating the number of times the level was completed, and goes to the `LevelCompleteCount` column.  
3. In the "**Add feature configuration to the game's schema**", you will add an extra field to the Feature dictionary:
  - "subfeatures" maps to a sub-dictionary.  
  Each element of the sub-dictionary has a key string, which we **strongly** recommend you match with the name(s) given in the corresponding Feature's  `Subfeatures()` function.  
