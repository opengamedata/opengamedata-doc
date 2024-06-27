## Getting Started

This document will help you to get up and running with the **`OpenGameData`**  tools.
There are a few use-cases that this documentation will address - please review the following section to determine which case you need, and advance to the corresponding sections.

### OpenGameData Use-Cases

I need to...

* [Retrieve a Dataset from my OpenGameData Data Source](#retrieve-a-dataset-from-my-opengamedata-data-source)
* [Integrate a new Game with OpenGameData](#integrate-a-new-game-with-opengamedata)
* [Integrate a new Data Source with OpenGameData](#integrate-a-new-data-source-with-opengamedata)
* [Perform Feature Engineering on my Raw Event Data](#perform-feature-engineering-on-my-raw-event-data)
* [Perform OpenGameData Processing in my 3rd-Party Tool](#perform-opengamedata-processing-in-my-3rd-party-tool)

#### Retrieve a Dataset from my OpenGameData Data Source

You should read the following portions of the documentation in order to retrieve a dataset:

1. Start by [installing OpenGameData](./installation.md).  
    * When choosing between a "Package" and "Local Source" install, you should choose the ["Package" option](./installation.md/#installation-as-package)
2. If the game whose data you want to retrieve has not previously been added to OpenGameData, you should instead begin with Step 2 of [integrating a new game](#integrate-a-new-game-with-opengamedata).
    * Follow the remaining steps as needed, and return to this spot.
3. Check your [OpenGameData configuration](./configurations.md)
4. Perform your [dataset export](./basic_exports.md)

#### Integrate a new Game with OpenGameData

You should read the following portions of the documentation in order to integrate a new game with OpenGameData.:

1. Start by [adding event logging](../logging/index.rst) to your game.  
    * This chapter will walk you through the process of using the logging client and server to capture event data, optionally setting up a new OpenGameData data source.
2. Next, [install OpenGameData](./installation.md).  
    * When choosing between a "Package" and "Local Source" install, you should choose the ["Local Source" option](./installation.md/#installation-as-local-source-copy)
3. 

#### Integrate a new Data Source with OpenGameData

#### Perform Feature Engineering on my Raw Event Data

#### Perform OpenGameData Processing in my 3rd-Party Tool

### Special Case: I am a Data Team Intern at Field Day Lab
