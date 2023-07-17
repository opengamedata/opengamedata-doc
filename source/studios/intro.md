# Introduction for Game Studios

This document outlines the steps necessary to integrate a new game with the Open Game Data infrastructure.

The OGD pipeline is made up of the following steps:

1. **Game Client Logging**
2. **Data Storage**
3. **Data Transformations**  
    a. New Event Detection  
    b. Feature Extraction  
    c. (*future work*) Model Training  
4. **Monthly Outputs**

We provide various Open Game Data packages for use in each step of this process; you may freely use these, or substitute your own solution in for a given step.
If you are only interested in event logging, the data transformations step may be ommitted from your project.

## Game Client Logging

In this step of the pipeline, the game sends out telemetry events to be captured in a database.
These events should be chosen to allow post-hoc reconstruction of gameplay.
Please see [Event Design](./event_design.md) for more information about choosing which events to log.
If your game already implements some form of event logging, you may continue to use your existing set of events.

To maintain consistency of data across games, we use a standardized schema for events.
If you plan to use the `opengamedata-unity` or `opengamedata-js-log` packages to aid in event logging, this schema will be implicitly enforced; if you use a custom logging solution, we highly recommend following the standardized schema for maximum compatibility.
Please see [Open Game Data Event Schemas](./event_schema.md) for more details.

Once you have determined a set of events and the data to be logged with them, you will need to implement logging within your game. We recommend using one of the Open Game Data packages to ease this process.
We currently have packages for Unity and JavaScript game projects.
To get started, please see the readme guide for the package appropriate for your game:
* For Unity: `opengamedata-unity` user's [guide](https://github.com/opengamedata/opengamedata-unity#readme)
* For JavaScript: `opengamedata-js-log` user's [guide](https://github.com/opengamedata/opengamedata-js-log#readme)

## Data Storage

Once you have implemented logging in your game, you'll need a database to receive and store the logged events.
