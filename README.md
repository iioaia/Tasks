# Tasks
Here is the complete development document for your web-based task tracker. The AI coding agent can use these specifications to generate the application.
Development Document: Project "Lighthouse" Task Tracker
1. Project Overview
Project Lighthouse is a lightweight, self-hosted task management application. It runs on a Windows PC and is accessible via a web browser to any device on the local network. The system is designed for a single user accessing it from multiple devices, with a focus on simplicity, real-time data synchronization, and ease of setup.
2. Technology Stack
 * Backend Framework: Python with Flask. Flask is a minimal and flexible web framework perfect for creating the API and serving the application.
 * Database: SQLite. A serverless, file-based database that is robust, portable, and requires no separate installation.
 * Frontend: Standard HTML5, CSS3, and JavaScript (ES6+). No complex frontend framework is required, ensuring simplicity and fast loading.
 * Deployment: A simple batch script (.bat) will handle the setup and launch process, including creating a Python virtual environment (venv) and installing dependencies.
3. Core Features & Functionality
3.1. User Interface (UI)
 * Layout: The UI will be a three-pane responsive web interface.
   * Left Pane (Projects): A scrollable list of all projects. The user can add, select, rename, and delete projects from here.
   * Middle Pane (Tasks): Displays the list of tasks for the currently selected project. The user can add, select, complete, and delete tasks.
   * Right Pane (Task Details): Shows the detailed view of the selected task, where all its properties can be edited.
 * Theme: A toggle switch will be present in the UI to switch between Light Mode and Dark Mode. The user's preference will be saved in the browser's local storage.
 * Real-time Sync / Auto-Save: There will be no manual "Save" button. Any change made in the UI (e.g., editing a task title, adding a tag, changing priority) will be automatically saved to the backend. This ensures that when the user switches devices, the data is always current.
3.2. Project Management
 * Create Project: Users can create new projects. Each project is essentially a container for tasks.
 * Modify Project: Users can rename existing projects.
 * Delete Project: Users can delete a project. This action will also delete all tasks associated with that project.
3.3. Task Management
 * Create Task: Users can add new tasks to a selected project.
 * Modify Task: In the detail pane, users can edit all attributes of a task.
 * Task Properties:
   * Title: A brief name for the task.
   * Description: A larger text area for notes and details.
   * Web Links: A section where users can add multiple web links. These links must be clickable and open in a new browser tab.
   * Pinned Documents: A section where users can add local file paths (e.g., C:\Users\You\Documents\spec.docx). A button next to the path should attempt to open the file using the file:/// protocol.
   * Priority: Users can assign a priority level to a task from a predefined list: Low, Medium, High, Urgent.
   * Status: Tasks can be marked as Open or Completed. Completed tasks should be visually distinct (e.g., grayed out with a strikethrough).
 * Delete Task: Users can permanently delete a task.
3.4. Tag Management
 * Global Tags: Tags are global and can be applied to any task in any project.
 * CRUD for Tags: The application will have a simple interface (e.g., a settings modal) to Create, Edit, and Delete tags. Each tag will have a name and an associated color.
 * Assign/Unassign Tags: In the task detail view, users can assign one or more existing tags to the task or remove them.
4. Application Architecture
4.1. Backend (Python/Flask)
The Flask application will serve the index.html file and provide a JSON-based REST API for all data operations.
API Endpoints:
| Method | Endpoint | Description |
|---|---|---|
| GET | /api/data | Get all projects, tasks, and tags in a single initial payload. |
| POST | /api/projects | Create a new project. |
| PUT | /api/projects/<id> | Rename a project. |
| DELETE | /api/projects/<id> | Delete a project. |
| POST | /api/tasks | Create a new task. |
| PUT | /api/tasks/<id> | Update any part of a task (title, priority, status, etc.). |
| DELETE | /api/tasks/<id> | Delete a task. |
| POST | /api/tags | Create a new tag. |
| PUT | /api/tags/<id> | Update a tag's name or color. |
| DELETE | /api/tags/<id> | Delete a tag. |
| POST | /api/tasks/<id>/tags | Assign a tag to a task. |
| DELETE | /api/tasks/<task_id>/tags/<tag_id> | Remove a tag from a task. |
4.2. Database (SQLite)
The database will be a single file named task_tracker.db.
Schema:
 * projects
   * id (INTEGER, PRIMARY KEY)
   * name (TEXT, NOT NULL)
 * tasks
   * id (INTEGER, PRIMARY KEY)
   * project_id (INTEGER, FOREIGN KEY to projects.id)
   * title (TEXT, NOT NULL)
   * description (TEXT)
   * priority (TEXT)
   * status (TEXT, default 'Open')
   * links (TEXT, stored as a JSON array of strings)
   * files (TEXT, stored as a JSON array of strings)
 * tags
   * id (INTEGER, PRIMARY KEY)
   * name (TEXT, UNIQUE, NOT NULL)
   * color (TEXT)
 * task_tags (Junction Table)
   * task_id (INTEGER, FOREIGN KEY to tasks.id)
   * tag_id (INTEGER, FOREIGN KEY to tags.id)
4.3. Frontend (HTML/CSS/JS)
The frontend will be a Single Page Application (SPA).
 * index.html: The main container for the app.
 * style.css: Contains all styling, including variables for the light and dark themes. A class on the <body> tag (e.g., dark-mode) will control the theme.
 * app.js: The core JavaScript file. It will handle:
   * Fetching initial data from the /api/data endpoint on page load.
   * Dynamically rendering the projects, tasks, and details in the three panes.
   * Attaching event listeners to all interactive elements (buttons, input fields, etc.).
   * Making API calls to the backend whenever data is changed by the user. For input fields, this should be triggered on the blur event (when the user clicks away) or after a short debounce interval.
   * Managing the UI state (e.g., which project and task are currently selected).
5. Setup and Execution
The project folder should be structured for ease of use. A user will only need to run one file to get started.
Folder Structure:
/task-tracker/
|-- run.bat
|-- requirements.txt
|-- /app/
|   |-- __init__.py
|   |-- database.py
|   |-- models.py
|   |-- routes.py
|   |-- /static/
|   |   |-- css/style.css
|   |   |-- js/app.js
|   |-- /templates/
|   |   |-- index.html

requirements.txt file:
Flask

run.bat Script Logic:
 * Check if a venv folder exists. If not, create it: python -m venv venv.
 * Activate the virtual environment.
 * Install dependencies from requirements.txt: pip install -r requirements.txt.
 * Run a Python script that initializes the SQLite database if it doesn't exist.
 * Start the Flask application: flask run.
 * The script should output the URL for the user to access, such as: * Running on http://127.0.0.1:5000 and Access on your network at http://[Your-Local-IP]:5000.
 * 
