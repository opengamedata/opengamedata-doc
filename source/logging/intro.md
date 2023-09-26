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
In principle, any table-based database system may be used to record Open Game Data events; in practice, we have ready support for MySQL as a short-term container for new events, and for BigQuery for long-term archival of older events. Logging to another database system may require some additional development effort.

If you wish to utilize our existing logging infrastructure to capture your game events, please [contact us](TODO) to discuss integration with our instance of the OGD logging system.

If you prefer to create your own instance of logging infrastructure, there are two major steps to complete.
These are:

1. Set up logging to MySQL short-term database
2. Set up automatic archival to BigQuery.

### Logging to MySQL

This major step can be accomplished by doing the following:

1. Set up a "logging" server with PHP and MySQL:  
    This server will host the logging service.
    Please see our [reference platform](../software_platform.md) documentation for recommended software versions.
2. Create a MySQL database to hold game events:  
    This database should contain a table for each game, with each table having columns and data types corresponding to the OGD schema
3. Set up an instance of `opengamedata-logger` package:  
    This repository contains PHP scripts to receive a request containing event data, and insert the data into a local database.
    The config file should be set with credentials for writing to the MySQL database.
    The [readme](https://github.com/opengamedata/opengamedata-logger#readme) may be useful for understanding how `opengamedata-logger` works.  
4. Direct game logging client to your server:  
    If you used `opengamedata-unity` or `opengamedata-js-log` for event logging, you can configure the package to make requests through your server's instance of the PHP scripts.

### Archiving to BigQuery

This step can be accomplished by doing the following:

1. Create a Google Cloud project to host game data
2. Create a BigQuery project:  
    This project should, in turn, have datasets for each game, and each dataset should have tables with columns and data types corresponding to the MySQL schema.
3. Use `opengamedata-automation` to automatically archive data from MySQL to BigQuery

## Data Transformations

### Event Detectors

### Feature Extractors

## Monthly Outputs