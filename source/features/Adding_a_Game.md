# Adding a Game

## Terminology

First, a list of terminology you should be familiar with (see Glossary page if you're unfamiliar with any of these):

- **Event**
- **Feature**

## Steps to add the Game

In order to add a new game to the feature extraction tool, complete the following steps:

### 1. Create JSON file

First, we must define some things about the data we are extracting.
We do this in a JSON file, under the **games/<GAME_ID>** folder.
The name of the JSON file should be the same as the game ID used in the database, by convention in all-caps.  
*For example*: For the "Wave" game, the database uses an app_id of `WAVES`, so we name the JSON schema file as **games/WAVES/WAVES.json**  

A JSON schema file has three elements:

- `events`:
  A description of the event-specific data encoded in each database row.
  This element should be a dictionary mapping names of events to sub-dictionaries defining the data in the events.

- `features`:
  A description of what features OGD should produce, given the events described above.
  This element should in turn contain two sub-elements:
  <!-- - `per_level`: This sub-element should be a dictionary mapping the names of per-level features to descriptions of how the features are calculated/used. -->
  - `aggregate`: This sub-element should be a dictionary mapping the names of features aggregated over a whole session to descriptions of how the features are calculated/used.
  <!-- - `per_game`: This sub-element should be a dictionary mapping the names of per-game features to descriptions of how the features are calculated/used. -->
  - `per_custom_count`: This sub-element should be a dictionary mapping the names of features which are repeated for some specific number of times to a subdictionary. This, again, has three elements:
    - `count`: The number of times the feature is repeated
    - `prefix`: A prefix to use to distinguish repeats of the feature in the output file
    - `desc`: A description of how the feature is calculated

- `level_range` (optional):
  You may optionally add the `level_range` element to your JSON schema, which must be a sub-dictionary with `min` and `max` as its elements.
  If you do so, you can then use `level_range` as the `count` for a per-count feature (more information in the "Adding a Feature" doc).

Below is a sample of JSON schema formatting:

```json
{
    "events": {
        "ARROW_MOVE_RELEASE": {
            "event_custom":"string",
            "begin_val":"float",
            "end_val":"float"
        }
    },

    "features": {
        "per_count": {
            "totalArrowMoves": {
                "enabled": true,
                "count":"level_range",
                "prefix": "lvl",
                "description": "arrow moves across a given level [count of 'ARROW_MOVE_RELEASE' events]"
            },
            "questionAnswered" : {
                "enabled": false,
                "count" : 4,
                "prefix": "QA",
                "events": [],
                "description" : "The answer the user gave to a given question (or -1 if unanswered)"
            },
        },
        "aggregate": {
            "AverageArrowMoves" :  {
                "enabled": true,
                "description":"totalArrowMoves averaged over all levels"
            }
        }
    },

    "level_range": { "min":0, "max":34 },
}
```

### 2. Create the feature Extractor class

This will be a Python class inheriting from the `Extractor` base class.
By convention, the class should use the database app_id as a prefix for the class name, but use CamelCase (even if the app_id is not formatted as such).  
*For example*: For the "Wave" game, we would name the extractor `WaveExtractor` (as opposed to `app_id + "Extractor` => `WAVEExtractor`).  

You should put your `Extractor` subclass in the **games/<GAME_ID>** folder alongside the schema.

The `Extractor` subclass *must* implement the following functions:

- `__init__(self, session_id, game_schema)`: At minimum, this function should call the super constructor.
`session_id` has the id of the session from which we are extracting data, and `game_schema` contains the data from the schema we defined in step 1.
If your Extractor base class needs to keep track of any extra data from the schema to pass to its individual Features, you should store that data into an instance variable here.

- `_loadFeature(self, feature_type, name, feature_args, count_index)`:
This function is responsible for creating instances of the individual features for the game.
The system will automatically figure out which instances, and how many of each, should be created based on the schema in step 1.
However, we still need some code to call the constructors for these Features.
The `_loadFeature` function will effectively be one giant `if-elif-else` block, with one case for each type of Feature created for the game.  
The `feature_type` parameter contains the "key" for a given feature in the schema.
The `name` will be the `feature_type` with any prefix added, as in the case of "custom count" features.
`feature_args` is the subdictionary for the feature in the schema (note, you do not need to check the "enabled" item here, that is done automatically).
Lastly, `count_index` is used for "custom count" features, and says which number in the "count" the newly-constructed feature instance will have.
For example, if your count was 3, then the first instance constructed will get a `count_index` of 0, the second will get 1, and the third will get 2.  
A sample `_loadFeature` is shown below:

```python
    def _loadFeature(self, feature_type:str, name:str, feature_args:Dict[str,Any], count_index:Union[int,None] = None) -> Feature:
        ret_val : Feature
        if feature_type == "AverageArrowMoves":
            ret_val = AverageArrowMoves.AverageArrowMoves(name, feature_args["description"])
        elif feature_type == "TotalArrowMoves":
            ret_val = TotalArrowMoves.TotalArrowMoves(name, feature_args["description"], count_index)
        elif feature_type == "QuestionAnswered":
            ret_val = QuestionAnswered.QuestionAnswered(name, feature_args["description"], count_index)
        else:
            raise NotImplementedError(f"'{feature_type}' is not a valid feature for Waves.")
        return ret_val
```

### 3. Register game in ExportManager

Next, we need to ensure ExportManager knows what games are available.
The ExportManager is the class responsible for, well, managing exports.
This is where we will register the existence of our new game's feature extractor.
Go to the `_prepareExtractor()` function in ExportManager.py, and add a case to the `if-elif-else` block, matching the game id to your new Extractor.

```python
    elif request.game_id == "WAVES":
        game_extractor = WaveExtractor
```

### 4. Implement Features

Lastly, you need to implement all of your game Feature classes.  
Each of your Feature subclasses should go in the **games/<GAME_ID>/extractors** folder.
For more details on how to write your features, see the "**Adding a Feature**" page.

### 5. Set up Game Package

In order to ensure all of your Features get imported in the correct places, we want to treat your game's folder as a Python package/module.
To do this, you should create an `__init__.py` file inside the **games/<GAME_ID>/extractors** folder.  
Inside this file, you should create a list named `__all__`, and put the names of all your Feature subclasses as strings in the list.
Then, write individual import statments for each Feature subclass.

An example is shown below:

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