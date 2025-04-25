---
title: Architecture
weight: 1
---
## Overview
This document provides a high-level infrastructure diagram and an overview of the various components, processes, and protocols relevant to the Test Automation tool.

## Core Components
![alt text](/static/architecture.png)

### Client
This refers to the user interface of a web application, which is the point of interaction between users and the software. It includes elements such as buttons, menus, forms, and graphics that allow users to navigate and utilize the functionalities of the application.

### Auth Server
This handles user authentication and authorization processes, ensuring that only authorized individuals have secure access. This includes verifying user identities through login credentials and granting access to specific resources based on predefined roles and permissions.

### Core Service
The core service is responsible for orchestrating various processes, implementing business logic, and providing API access. This microservice efficiently manages requests related to configuration management, which includes:
- Creating and managing projects
- Defining steps within those projects
- Establishing assertions for testing purposes
- Designing test flows
- Generating datasets necessary for testing and analysis

By centralizing these functions, this service ensures a streamlined and cohesive approach to handling user requests and enhancing the overall application performance.

### Execution Dispatcher
The Execution Dispatcher is responsible for managing the submitted executions. It uses Kafka to queue the executions when multiple tasks are in progress. Additionally, it handles the logic for distributing the execution to different types of steps. During this process, Hazelcast’s cache is utilized to store the state of each execution.

### SQL Executor, Batch Executor, API Executor, UI Executor
These microservice components are responsible for establishing a connection to a target system and executing various testing operations. Each component is designed to handle specific aspects of the testing process. By integrating with the target system, these components facilitate data exchange and verification.

### Kafka Streams
A stream processing platform used for real-time data processing, enabling users to analyze and act on streaming data as it flows in. This platform facilitates the immediate collection, processing, and analysis of data from various sources, ensuring that insights are generated in the moment. Furthermore, it provides timely feedback to end users while the execution is ongoing, allowing for quick adjustments and responses to changing conditions.

### Hazelcast
An in-memory data grid that is used for caching information and managing distributed data storage. It allows for quick access to data by storing it directly in the system’s memory.

### PostgreSQL
A relational database that is used for the storage and management of persistent data. It organizes data into structured formats, typically utilizing tables, which allow for efficient retrieval and management.

### MinIO Object Storage
An object storage solution designed for efficiently storing large files and media assets that are produced during user interface (UI) execution or report generation processes.
