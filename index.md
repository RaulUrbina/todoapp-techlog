# Breakable Toy I - ToDo App
## Overview

The following document explains the development process of the first Breakable Toy for the Encora's Spark program, a ToDo App to manage tasks. This project was built on ReactJS + Typescript for the frontend and Java with Spring Boot for the backend.

## Context
The project key features contain the capability of:
- Creation, edit and deletion of tasks.
- Each task contains a name, priority and optionally a due date.
- A filter component to search by name, priority and status (done/undone)
- On the table, the users are able to check/uncheck, sort by prority or due date.
- A stats component where the average time of completion is displayed and stats filtered by priority (LOW, MEDIUM or HIGH).


### Technologies Used

#### Frontend
- ReactJS as framework
- Typescript as transpiler to have strict type.
- Tailwind as CSS framework
- Shadcn/ui for the components component, which is built using RadixUI as base library.
- Zustand as state manager.
- Axios for api requests
- Vistest as testing tool

### Backend
- Spring Boot as a framework
- Lombok to simplify que DTOs and Object structure
- Mockito for the unit test related with request

## Solution

### Backend
The backend follows the standard layered architecture:
1. **Controller**: Handles incoming HTTP requests and returns responses.
2. **Service**: Contains business logic and processes data from the controller.
3. **DTO (Data Transfer Objects)**: Transfers data between layers, especially from the controller to the service.
8. **Utils**: Contains utility classes or helper methods used across the application.

![Backend Folder Tree](https://i.ibb.co/sRKRbnR/Screenshot-2024-10-23-at-11-15-35-p-m.png)

#### Endpoints
The following endpoints have been implemented to handle all the necessary tasks operations:
- `POST /todos`: Create a new task.
- `GET /todos`: Retrieve all tasks with optional filters like priority, text, and completion status.
- `PUT /todos/{id}`: Update a task by its ID.
- `DELETE /todos/{id}`: Delete a task by its ID.
- `POST /todos/{id}/done`: Mark a task as completed.
- `PUT /todos/{id}/undone`: Mark a task as incomplete.
- `GET /todos/completion-stats`: Retrieve the stats for the done done tasks.

#### Unit Tests
##### Tools Used
- **JUnit 5:** For structuring and running the tests.
- **Mockito:** To mock service dependencies and simulate the external behavior.
##### ToDoServiceTest:
- `testAddItem_Success`: Tests that a task is correctly added to the list and that the task’s properties (text, priority, completion status) are properly set.
- `testGetItem_NotFound`: Tests that when trying to retrieve a non-existing task, the service correctly returns an empty result.
- `testMarkAsDone_Success`: Ensures that a task is marked as completed and the completion date is properly set.
- `testUpdateItem_Success`: Verifies that updating a task changes the task’s text and priority while keeping its ID intact.
##### ToDoControllerTest:
- `testGetItem_Success`:  Ensures that the controller correctly retrieves a task and returns a 200 OK status.
- `testAddItem_Success`: Verifies that a new task is successfully created and returned with a 201 CREATED status.
- `testDeleteItem_Success`: Ensures that when a task is deleted, the service responds with an appropriate success message.
- `testDeleteItem_NotFound`: Tests that when trying to delete a non-existing task, the controller responds with a 404 NOT FOUND status.

