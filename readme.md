# Thymio Platooning and Give Away - Software Requirements Specification (SRS)

## Introduction
The Software Requirements Specification (SRS) outlines the product, Thymio Platooning and Give Away, and its release version, v1. This document delineates the scope of the project, which involves coordinating five Thymio robots to navigate a crossroad simultaneously, following a predetermined route without collisions. 

## References
- **Title**: Model-Based Systems Engineering Approach to Autonomous Driving  
  **Author**: SARANGI VEERMANI LEKAMANI  
  **Source**: [PDF](http://kth.diva-portal.org/smash/get/diva2:1335887/FULLTEXT01.pdf)

- **Title**: Use case  
  **Author**: Davide Rossi  
  **Source**: [PDF](http://sweng.web.cs.unibo.it/wp-content/uploads/2015/09/Use-case.pdf)

## Overall Description
### Product Functions
- **SOS Communication**: Thymio robots must halt their current actions and respond to emergency situations upon receiving an SOS command.
- **Route Calculation**: Thymio robots should adhere to a predefined route, recalculating if necessary to avoid collisions with other robots or obstacles.
- **Obstacle Detection and Avoidance**: Thymio robots are tasked with identifying obstacles and taking evasive action to prevent collisions.
- **Intercommunication**: Utilizing the RUMI protocol, Thymio robots prioritize actions to facilitate smooth interaction.
- **Stop At Destination**: Thymio robots must recognize when they reach their destination and cease movement.
- **Emergency Response**: The system can detect and alert other robots of emergency situations, enhancing safety.

<image>

## Requirements
### Use Case Diagram

<image>


# Thymio Robot System README

## High-Level Use Cases

| Use case                        | Actor                       | Description                                                                 |
|---------------------------------|-----------------------------|-----------------------------------------------------------------------------|
| UC001_SlowDownVehicle           | Obstacle                    | When Thymio detects any obstacle it slows down                              |
| UC002_Start                     | Moving Thymio               | When Thymio receives the start command it will start                        |
| UC003_MoveOnAlternatePath       | Wall, Stationary Thymio     | When Thymio detects a wall or another thymio it changes its path           |
| UC004_FollowObsticaleUntilSamePath | Front Same Direction Thymio | When Thymio B detects Thymio A in front and not facing it, B follows A until both shares same path |
| UC004_FollowObstacleUntilSamePath (Sleep) | Front Same Direction Thymio | When Thymio B detects another Thymio A in front of it and A is not facing B, Thymio B follows Thymio A until both shares the same path. If Thymio A stops, then Thymio B will enter a sleep state |
| UC005_Sleep                     | Front Opposite Direction Thymio | When Thymio B detects another Thymio A in front of it and B is facing A, Thymio B will enter into a sleep state |
| UC005_Sleep (Wait till Cross)   | Front Opposite Direction Thymio | When Thymio B detects another Thymio A in front of it and B is facing A, Thymio B will enter into a sleep state then thymio B will wait until A cross |
| UC006_DestinationReached        | Cylinder                    | When Thymio detects cylinders this means it has reached a destination        |

## Expanded Use Cases

### UC004_FollowObstacleUntilSamePath (Sleep)

| Use case                        | Actor                       | Description                                                                 |
|---------------------------------|-----------------------------|-----------------------------------------------------------------------------|
| UC004_FollowObstacleUntilSamePath (Sleep) | Front Same Direction Thymio | Thymio B is moving forward and detects another Thymio A in front of it. Thymio A is not facing Thymio B and therefore starts to follow Thymio A's movement until they are on the same path. Thymio B keeps following Thymio A until they are on the same path or until Thymio A stops moving. If Thymio A stops, Thymio B will enter a sleep state until Thymio A resumes its movement. Thymio B will continue to follow Thymio A until they share the same path, or until Thymio A stops again, at which point Thymio B will enter a sleep state again. When thymio B requires to change the path it will stop following thymio A

- **Precondition**: Thymio A and Thymio B are powered on and in operational mode. Thymio B is moving forward and able to detect obstacles in front of it. Thymio A is within range of Thymio B's sensors.
- **Postcondition**: Thymio B is either following Thymio A or in a sleep state.
- **Minimal Guarantees**: Thymio B will follow Thymio A until they share the same path, or until Thymio A stops moving. If Thymio A stops moving, Thymio B will enter a sleep state until Thymio A resumes its movement.
- **Success Guarantees**: Thymio A and Thymio B will continue to operate normally even after the completion of this use case. Thymio B will follow Thymio A until they are on the same path, ensuring that the two thymios stay together and avoid collisions with other objects.

### UC004_FollowObstacleUntilSamePath (Sleep) Alternate Path

- If Thymio A stops, Thymio B enters a sleep state until Thymio A resumes its movement. Thymio B does not resume its movement and the scenario ends.

### UC004_FollowObstacleUntilSamePath (Sleep) Trigger

- The Thymio robot in front of Thymio B that is not facing it.

## System Features

### Architecture

#### SoS structure and rules:

| ID            | Description                                                                                  |
|---------------|----------------------------------------------------------------------------------------------|
| REQ-SOS-01    | The SoS shall be composed of 5 Thymios                                                       |
| REQ-SOS-02    | At SoS starts, the Thymios shall be positioned at every entry road must be occupied by one thymio |
| REQ-SOS-03    | At the beginning of every entry road must be occupied by one thymio                         |
| REQ-SOS-04    | The SoS target shall be when all Thymios reach their respective destinations without any collision and error |
| REQ-SOS-05    | The execution shall complete when all thymios reach their destination                        |

#### Thymio Specification and Rules:

| ID            | Description                                                                                  |
|---------------|----------------------------------------------------------------------------------------------|
| REQ-TYM-01    | Each Thymio shall have hardcoded the following information: Unique identifier, Group information, Sensor and actuator configurations, path, position, destination |
| REQ-TYM-02    | Thymio must be able to estimate the distance from other Thymio from the environment with the help of 5 front Proximity sensor |
| REQ-TYM-03    | Thymio must be able to estimate the relative distance from the wall of the environment with the help of 2 back Proximity sensors |
| REQ-TYM-04    | Each Thymio must be able to reach its destination without collision and violation of regulation |
| REQ-TYM-05    | Each state of the thymio must show different colours the thymio                               |
| REQ-TYM-06    | Thymio may operate in one of at least six states. i.e Forward, Reverse, left, right, stop, finished and sleep |
| REQ-TYM-07    | Thymio should receive a signal to start                                                        |
| REQ-TYM-08    | A Thymio shall operate to a black line                                                         |

#### Some requirements on Environments:

| ID            | Description                                                                                  |
|---------------|----------------------------------------------------------------------------------------------|
| REQ-EVN-01    | All Thymios should operate in a space of at least 140x140 cm Environment                      |
| REQ-EVN-02    | The surface shall be smooth, flat, and free from any unnecessary obstacles                     |
| REQ-EVN-03    | All The 5 two-ways road must be represented by a black line.                                   |
| REQ-EVN-04    | Cylinders must show the destination                                                           |

### Communication/RUI

#### Communication at CS-level:

| ID            | Description                                                                                  |
|---------------|----------------------------------------------------------------------------------------------|
| REQ-CCS-01    | Each Thymio must detect other obstacles using RUPI                                              |

#### Communication at SoS-level:

| ID            | Description                                                                                  |
|---------------|----------------------------------------------------------------------------------------------|
| REQ-CSOS-01   | All thymios must receive start command from SOS using RUMI before moving                        |
| REQ-CSOS-02   | Each Thymio must exchange priority using RUMI                                                  |
| REQ-CSOS-03   | All thymios must be able to receive stop command from SOS using RUMI in an emergency situation |

## Test Cases

| Test Case ID   | Test Scenario                                                             | Test Steps                                                                                 | Test Data                  | Expected Results         | Actual Results   | Pass/Fail |
|----------------|---------------------------------------------------------------------------|---------------------------------------------------------------------------------------------|----------------------------|--------------------------|------------------|-----------|
| TU01           | Slow Down Vehicle When Obstacle Detected                                  | 1. The obstacle is Placed in front of moving thymio                                           | Obstacles = Wall, Other Thymio | Thymio must Slow down    | As Expected      | Pass      |
| TU02           | Don’t Slow Down Vehicle When Obstacle is not Detected                     | 1. The obstacle is not Placed in front of moving thymio                                       | Obstacle = None            | Thymio must not slow down | As Expected      | Pass      |
| TU03           | Global event Start with code 100                                          | 1. Receive the Start command from SOS                                                          | GlobalEvent = Start 100    | Thymio must start moving  | As Expected      | Pass      |
| TU04           | Global event Start with invalid                                           | 1. Receive the Start command from SOS                                                          | GlobalEvent = start 100    | Thymio must not start moving | As Expected      | Pass      |
| TU05           | Move On Alternate Path when detecting stationary obstacle                | 1. The obstacle is Placed in front of moving thymio                                           | Obstacles = Wall, Stationary Thymio | Thymio Should Change its path | As Expected      | Pass      |
| TU06           | Don’t Move On Alternate Path if no obstacle detected                      | 1. The obstacle is not Placed in front of moving thymio                                       | Obstacle = None            | Thymio Should not Change its path | As expected to   | Pass      |
| TU07           | Follow Obsticale Until Same Path                                         | 1. Thymio B detects thymio A in front of it                                                    | Obstacle= Front Same Direction Thymio | Thymio B Starts Following Thymio A Until Thymio B and A share the same path | As expected to   | Pass      |
| TU07-B         | Thymio A stops State                                                      | 1. Thymio B detects thymio A in front of it                                                    | Obstacle= Front Same Direction Thymio, StateThymioA= Stops | If A==  stops Then B ==  Sleep | As expected to   | Pass      |
| TU07C          | Thymio A Forward State                                                    | 1. Thymio B detects thymio A in front of it                                                    | Obstacle= Stationary Thymio, State ThymioA= Forward | If A == Forward then B == Forward | As expected to   | Pass      |
| TU08           | Sleep when Front facing Opposite Direction Thymio                        | 1. Thymio B detects thymio A in front of it                                                    | Obstacle= Front Opposite Direction Thymio | StateB == Sleep          | As expected to   | Pass      |
| TU08B          | Wait till Cross                                                           | 1. Thymio B detects thymio A in front of it                                                    | Obstacle= Front Opposite Direction Thymio | Thymio B will wait till thymio A crosses | As expected to   | Pass      |
| TU08C          | Start after Wait till Cross                                               | 1. Thymio B detects thymio A in front of it                                                    | Obstacle = None            | stateB == forward         | As expected to   | Pass      |
| TU09           | Destination Reached                                                       | 1. Thymio B detects cylinder on front sensor                                                   | Obstacle = cylinder        | stateB == finished        | As expected to   | Pass      |
| TU09B          | Destination Reached                                                       | 1. Thymio B detects another Thymio A in stop state                                             | Obstacle = None            | journeyEnd =100, stateB == finished | As expected to   | Pass      |

## An overview of the ResilBlockly model

### Introduction

This report provides an introduction to the key components and requirements of the ResilBlockly Model, developed using blockly4sos.

### Main Features

- **Environment**: Includes Road, Road Marking, Cylinder, Wall, and Moving Obstacle.
- **Relied upon Message Interface (RUMI)**: Facilitates communication among CS components.
- **SOS**: Defines Thymio behaviour, state space, and services.
- **Role player**: Human tap interacts with the Thymio robot.
- **Physical System**: Comprises Proximity Sensor, Ground Sensor, LED Lights, Motors, and Buttons.
- **Relied upon Physical Interface (RUPI)**: Facilitates physical sensing.

### Conclusion

The ResilBlockly Model is developed on blockly4sos, integrating various components and interfaces for effective system functioning.

## Discussion on the simulated execution

### Introduction

This document explains the setup and priority settings for Thymio robots in the Aseba Studio simulator playground.

### Priority Configuration

- **Priority 1**: Thymio reverses when detecting an obstacle.
- **Priority 2**: Thymio continues moving even if an obstacle is detected.
- **Priority 0**: Thymio stops until the obstacle is gone.

### Configuration Summary

#### With Priority 1 and 2:

| Thymio | Priority | Behaviour                                        |
|--------|----------|--------------------------------------------------|
| T1     | 1        | Reverses when an obstacle is detected            |
| T2     | 2        | Continues moving even if an obstacle is detected |
| T3     | 1        | Reverses when an obstacle is detected            |
| T4     | 2        | Continues moving even if an obstacle is detected |
| T5     | 1        | Reverses when an obstacle is detected            |

#### With Priority 0:

| Thymio | Priority | Behaviour                                    |
|--------|----------|----------------------------------------------|
| T1     | 2        | Continues moving even if an obstacle is detected |
| T2     | 2        | Continues moving even if an obstacle is detected |
| T3     | 1        | Reverses when an obstacle is detected        |
| T4     | 1        | Reverses when an obstacle is detected        |
| T5     | 0        | Stops the Thymio until the obstacle is gone |

### Conclusion

The priority settings ensure effective collision avoidance and deadlock prevention during simulated execution.


