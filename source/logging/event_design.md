# Event Design

The process of identifying and designing the data for events in a game is very much dependent on the specifics of the game.
For example, the events in a rhythm-based VR game tend to be rather different from a 2D turn-based strategy game.
However, there are some common best practices.

In general, event design can be performed with the following process:

1. Identification
2. Specification
3. Implementation
4. Iteration

## Event Identification

The first step in event design is to identify the individual types of events that occur within gameplay.
[Owen and Baker](https://link-springer-com.ezproxy.library.wisc.edu/article/10.1007/s10758-018-9393-9) identify three main categories of event: **Player Actions**, **System Feedback**, and **Progression**.
This is a useful typology to reference when identifying in-game events.

In this step of the event design process, a gameplay session should be scheduled, with a game engineer and a data scientist present.
One attendee should play through as much of the game as is necessary to interact with each unique game mechanic or system behavior, while all other attendees observe the gameplay.
When a new type of interaction between the player and game is observed, it should be named and recorded.
One attendee should act as notetaker, recording these names and a very brief description of each event type.
Frequent pauses may be required as attendees discuss the game mechanics and the events they observe.

The attendees should look for events within each category of the Owen and Baker typology.
A brief summary of each category is given below:

* **Player Actions** : events where the player directly performs an action within the game, such as clicking or dragging a UI element, moving within the game world, etc.
  **Player Actions** should generally be given names in a `verb_noun` format, such as `start_game` or `drag_item`, i.e. using an "active voice."
* **System Feedback** : events where the game system provides feedback indicating the results of **Player Actions**.
  These might include displaying a popup window, playing a dialog file, or updating a scoreboard.
  **System Feedback** events should be named with a `noun_verb` format, such as `score_updated` or `dialog_bubble_displayed`, i.e. using a "passive voice."
* **Progression** : these are events indicating a concrete progression of the player within the system of the game.
  These might include completing a puzzle, quest, or level, or completing an achievement.
  We do not have a particular naming convention for **Progression** events; either a `verb_noun` or `noun_verb` format is reasonable, so long as they are used consistently.

## Event Specification

Once the distinct types of event in the game have been identified and briefly described, the specific details of the data for each event type should be specified.
Some discussion of data details is expected as part of the event identification process, and some preliminary plans for the data details may even be recorded in the "brief description" of each event.

However, a data scientist on a given project should be assigned to formally specify the data members of each event.
This documentation can then be handed off to a game engineer in the _Event Implementation_ step.

The OpenGameData standard event schema has a few "compound" columns for "contextual" data.
Namely, these are the `EventData`, `GameState`, and `UserData` elements.
These are the elements that need to be specified during event design; other elements (such as identifiers, timing, and versioning) are defined across all games, and the only work here is to determine which identifiers are used by the game, and what versioning scheme the game uses.
In addition, these three elements have a "dictionary" type, so each is formed as a collection of key-value pairs.
Thus, for each game, the collection of keys and corresponding values should be determined as a part of event design.

### `UserData`

This element is for data about the specific user playing the game, and should have the same form (i.e. the same set of keys) across all event types in the game.
Sub-elements in `UserData` might include a permanent user ID from a cross-game sign-in system, a count of prior plays, or a current ranking in a multiplayer game.

For example:

```json
{
   "system_user_id" : "Joe Player",
   "play_count"     : 5,
   "world_rank"     : 42
}
```

### `GameState`

This element is for data about the state of the game at the instant an event occurred, which contextualizes the event.
By "at the instant an event occurs," we mean the `GameState` should 
Like `UserData`, the set of keys in `GameState` should be constant across all event types, within a given game.
Sub-elements within `GameState` might include the current level, the current score, or the set of items a player has collected.

For example:

```json
{
   "level" : 5,
   "score" : 152,
   "collected_items" : ["SWORD", "SHIELD", "GEMSTONE"]
}
```

In general, most `GameState` data could simply be inferred from past events.
For example, for any given event, the current level could be determined by looking for the most recent `new_level` event (or whatever name is chosen).
Thus, there is a tension between what data is useful to include in the `GameState`, and what context should simply be calculated as needed from past event data.
As a general heuristic, we include data that we believe to be useful for contextualizing all (or most) events in a given game (e.g. the current level), and exclude data that is only useful in specific cases (e.g. the number of attempts at a difficult task that exists only in level 5), leaving it to be inferred during post-hoc data analysis.

### `EventData`

## Event Implementation

## Iteration

In general, event design is an iterative process.


## Best Practices & Naming Conventions

Below, we list some recommended practices and naming conventions.
These are, in general, arbitrary but reasonable, and are recommended to create consistency across game projects.

### Event Names

* Formatting:  
  We use a `snake_case` format for event names.
  (This is arbitrary and kinda dumb as a `PascalCase` or `CONSTANT_CASE` format would be far more consistent with our other coding conventions, where classes/types use `PascalCase`, and enums use `CONSTANT_CASE`, but historically we used `snake_case` for almost all names in SQL. C'est la vie.)
* Name form:  
  All event types should be named with an object and a verb, with the order of the two used to help distinguish the event category.
  **Player Actions** should have an "active" `verb_object` form, with a present-tense verb, such as `click_button`.
  **System Feedback** events should have a "passive" `object_verb` form, with a past-tense verb, such as `dialog_displayed`.

### Context Data Elements

* Formatting:
  We use a `snake_case` format for all keys in the context elements (`EventData`, `GameState`, and `UserData`).
  This is consistent with our general coding conventions, where variables use `snake_case` formatting.
