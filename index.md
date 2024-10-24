<link rel="stylesheet" href="/style.css">

# **Breakable Toy I - ToDo App - Technical Log**
## **Overview**

The following document explains the development process of the first Breakable Toy for the Encora's Spark program, a ToDo App to manage tasks. This project was built on ReactJS + Typescript for the frontend and Java with Spring Boot for the backend.

## **Context**
The project key features contain the capability of:
- Creation, edit and deletion of tasks.
- Each task contains a name, priority and optionally a due date.
- A filter component to search by name, priority and status (done/undone)
- On the table, the users are able to check/uncheck, sort by prority or due date.
- A stats component where the average time of completion is displayed and stats filtered by priority (LOW, MEDIUM or HIGH).


### **Technologies Used**

#### **Frontend**
- ReactJS as framework
- Typescript as transpiler to have strict type.
- Tailwind as CSS framework
- Shadcn/ui for the components component, which is built using RadixUI as base library.
- Zustand as state manager.
- Axios for api requests
- Vistest as testing tool

#### **Backend**
- Spring Boot as a framework
- Lombok to simplify que DTOs and Object structure
- Mockito for the unit test related with request

## **Solution**

### **Backend**
The backend follows the standard layered architecture:
1. **Controller**: Handles incoming HTTP requests and returns responses.
2. **Service**: Contains business logic and processes data from the controller.
3. **DTO (Data Transfer Objects)**: Transfers data between layers, especially from the controller to the service.
8. **Utils**: Contains utility classes or helper methods used across the application.

![Backend Folder Tree](https://i.ibb.co/sRKRbnR/Screenshot-2024-10-23-at-11-15-35-p-m.png)

#### **Endpoints**
The following endpoints have been implemented to handle all the necessary tasks operations:
- `POST /todos`: Create a new task.
- `GET /todos`: Retrieve all tasks with optional filters like priority, text, and completion status.
- `PUT /todos/{id}`: Update a task by its ID.
- `DELETE /todos/{id}`: Delete a task by its ID.
- `POST /todos/{id}/done`: Mark a task as completed.
- `PUT /todos/{id}/undone`: Mark a task as incomplete.
- `GET /todos/completion-stats`: Retrieve the stats for the done done tasks.

#### **Unit Tests**
##### **Tools Used**
- **JUnit 5:** For structuring and running the tests.
- **Mockito:** To mock service dependencies and simulate the external behavior.
##### **ToDoServiceTest**:
- `testAddItem_Success`: Tests that a task is correctly added to the list and that the task’s properties (text, priority, completion status) are properly set.
- `testGetItem_NotFound`: Tests that when trying to retrieve a non-existing task, the service correctly returns an empty result.
- `testMarkAsDone_Success`: Ensures that a task is marked as completed and the completion date is properly set.
- `testUpdateItem_Success`: Verifies that updating a task changes the task’s text and priority while keeping its ID intact.
##### **ToDoControllerTest**:
- `testGetItem_Success`:  Ensures that the controller correctly retrieves a task and returns a 200 OK status.
- `testAddItem_Success`: Verifies that a new task is successfully created and returned with a 201 CREATED status.
- `testDeleteItem_Success`: Ensures that when a task is deleted, the service responds with an appropriate success message.
- `testDeleteItem_NotFound`: Tests that when trying to delete a non-existing task, the controller responds with a 404 NOT FOUND status.
#### **Postman API Tests**
To ensure that the API was working correctly, a collection of Postman requests was created. Thanks to this tool the testing of the system was easier.

![image](https://github.com/user-attachments/assets/b098cff5-295d-4adf-8bdd-d156a202a6e7)





### **Frontend**

#### **Layout**
The layout was implemented using a mix of flexbox and grid layouts. Grid was used for the filter component since in my opinion, is more flexible when asymmetrical components are implemented, which is the case for the filter. Here is the abstraction made:

![image](https://github.com/user-attachments/assets/e8e0be07-d885-4f1d-9b05-5ba800bdb260)



After figuring out the main components, implementing the layout got a lot easier. To help with the components, I decided to use the collection from shadcn/ui. It's worth mentioning that this isn't a traditional library like MaterialUI. The cool thing about shadcn is that it gives you more control over the components. Instead of being stuck with predefined elements, you import the actual source code into your project, which means you can fully customize them. They’re based on RadixUI, but since they become part of your project, you aren’t tied down by external dependencies. With this approach, the frontend ended up looking like this:

![image](https://github.com/user-attachments/assets/ad51e345-0a30-440b-a68f-c0c7b043dffb)

#### **The Logic Behid the Frontend**

##### **Project Structure**

The folder structure we used in the project can be considered a feature-based or modular component architecture. 
Basically, it organizes files based on their role or function in the app, which helps a lot with code readability, maintenance, and keeping things clean as the project grows.

![image](https://github.com/user-attachments/assets/90661f3f-bc59-4320-8e7a-7d4f58afdfa1)

Here’s a general explanation:


- **Root Files**: You’ll find files like `App.tsx`, `App.css`, and `main.tsx`. These are the main entry points of the app. They include things like the primary app component and some global styles.
- **components**:
  - **shared**: This is where the reusable components are stored. For the most complex components are subfolders, to keep a clean structure on the main component.
  - **ui-library**: Here, we have all the low-level UI elements like `button`, `card`, `input`, `select`, and more. These are the previously mention shadcn/ui components.
- **hooks**: This folder contains custom hooks for managing the app's logic and state. You'll find hooks like `useModal` for modal management and `useToDoStore` for the centralized communication of the components, we'll get deeper on this on the next section.
  - **ui-library**: The hooks for the shadcn/ui components are stored here.
- **interfaces**: Since we are using TypeScript, this folder defines all the types and interfaces like `ToDo`, `ToDoParams`, and `ToDoPayload` to ensure type safety throughout the app.
- **services**: The `ToDoService.ts` file is stored here, which handles all the API calls and communication between the front end and back end.

#### **State Manager**

Since i have worked on a couple of JavaScript based projects before, I'm very familiar of the well known "Prop Drilling" issue, therefore i decided to base all the communication of my project using state managers so i avoid passing data as props for each components and relying on events to trigger callbacks and update others. Two state managers were created, the main one is the ```useToDoStore.ts```, where i centralized the communcation between components.
Thanks to this file, when a new task is created, the data doesn't need to be passed back and forth between components. Instead, the ```useToDoStore``` manages the state globally, and any component that depends on this state can automatically reflect the changes. For instance, when a task is marked as done, the toggle function updates the task's status in the global state and re-fetches the stats to ensure the entire application reflects the latest data.




