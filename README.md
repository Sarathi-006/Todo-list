# Ex03 To-Do List using JavaScript

## Date: 17/08/2026

## AIM

To create a To-do Application with all features using JavaScript.

## ALGORITHM

### STEP 1

Build the HTML structure (index.html).

### STEP 2

Style the App (style.css).

### STEP 3

Plan the features the To-Do App should have.

### STEP 4

Create a To-do application using JavaScript.

### STEP 5

Add functionalities such as adding, completing, deleting, and clearing tasks.

### STEP 6

Test the App.

### STEP 7

Open the HTML file in a browser to check layout and functionality.

### STEP 8

Fix styling issues and refine content placement.

### STEP 9

Deploy the website.

### STEP 10

Upload to GitHub Pages for free hosting.

## PROGRAM

### index.html

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>TaskFlow - To-Do App</title>
    <link rel="stylesheet" href="style.css">
</head>
<body>

    <div class="app-container">
        <header>
            <h1>TaskFlow</h1>
            <p>Organize your day. Get things done.</p>
        </header>

        <main>
            <div class="input-section">
                <input type="text" id="taskInput"
                       placeholder="Enter a new task...">
                <button id="addTaskBtn">Add Task</button>
            </div>

            <div class="task-info">
                <span id="taskCount">0 tasks</span>
                <button id="clearCompleted">
                    Clear Completed
                </button>
            </div>

            <ul id="taskList"></ul>

            <p id="emptyMessage">
                No tasks yet. Add your first task!
            </p>
        </main>

        <footer>
            <p>
                Designed by <strong>YOUR NAME</strong> |
                Register No: <strong>YOUR REGISTER NUMBER</strong>
            </p>
        </footer>
    </div>

    <script src="script.js"></script>
</body>
</html>
```

### style.css

```css
* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
}

body {
    font-family: Arial, sans-serif;
    min-height: 100vh;
    background: linear-gradient(135deg, #667eea, #764ba2);
    display: flex;
    justify-content: center;
    align-items: center;
    padding: 20px;
}

.app-container {
    width: 100%;
    max-width: 650px;
    background: #ffffff;
    border-radius: 18px;
    box-shadow: 0 15px 40px rgba(0, 0, 0, 0.2);
    overflow: hidden;
}

header {
    background: #4f46e5;
    color: white;
    text-align: center;
    padding: 30px 20px;
}

header h1 {
    font-size: 2.3rem;
    margin-bottom: 8px;
}

header p {
    font-size: 1rem;
}

main {
    padding: 30px;
}

.input-section {
    display: flex;
    gap: 10px;
}

#taskInput {
    flex: 1;
    padding: 14px;
    border: 2px solid #ddd;
    border-radius: 8px;
    font-size: 1rem;
    outline: none;
}

#taskInput:focus {
    border-color: #4f46e5;
}

#addTaskBtn {
    padding: 14px 20px;
    border: none;
    border-radius: 8px;
    background: #4f46e5;
    color: white;
    cursor: pointer;
    font-weight: bold;
}

#addTaskBtn:hover {
    background: #3730a3;
}

.task-info {
    display: flex;
    justify-content: space-between;
    margin: 25px 0 15px;
    color: #555;
}

#clearCompleted {
    border: none;
    background: transparent;
    color: #4f46e5;
    cursor: pointer;
    font-weight: bold;
}

#taskList {
    list-style: none;
}

#taskList li {
    display: flex;
    justify-content: space-between;
    align-items: center;
    gap: 15px;
    padding: 15px;
    margin-bottom: 10px;
    background: #f7f7fb;
    border-radius: 10px;
    border-left: 4px solid #4f46e5;
}

.task-content {
    display: flex;
    align-items: center;
    gap: 12px;
}

.task-content span {
    word-break: break-word;
}

.task-content input {
    width: 18px;
    height: 18px;
    cursor: pointer;
}

.completed .task-content span {
    text-decoration: line-through;
    color: #999;
}

.delete-btn {
    border: none;
    background: #ef4444;
    color: white;
    padding: 8px 12px;
    border-radius: 6px;
    cursor: pointer;
}

.delete-btn:hover {
    background: #dc2626;
}

#emptyMessage {
    text-align: center;
    color: #999;
    padding: 25px 0;
}

footer {
    background: #f3f4f6;
    text-align: center;
    padding: 18px;
    color: #555;
}

footer strong {
    color: #4f46e5;
}

@media (max-width: 600px) {
    .input-section {
        flex-direction: column;
    }

    #addTaskBtn {
        width: 100%;
    }

    .task-info {
        flex-direction: column;
        gap: 10px;
    }
}
```

### script.js

```javascript
const taskInput = document.getElementById("taskInput");
const addTaskBtn = document.getElementById("addTaskBtn");
const taskList = document.getElementById("taskList");
const taskCount = document.getElementById("taskCount");
const emptyMessage = document.getElementById("emptyMessage");
const clearCompleted = document.getElementById("clearCompleted");

let tasks = JSON.parse(localStorage.getItem("tasks")) || [];

function saveTasks() {
    localStorage.setItem("tasks", JSON.stringify(tasks));
}

function renderTasks() {
    taskList.innerHTML = "";

    emptyMessage.style.display =
        tasks.length === 0 ? "block" : "none";

    tasks.forEach((task) => {
        const li = document.createElement("li");

        if (task.completed) {
            li.classList.add("completed");
        }

        li.innerHTML = `
            <div class="task-content">
                <input type="checkbox"
                    ${task.completed ? "checked" : ""}>
                <span>${escapeHTML(task.text)}</span>
            </div>
            <button class="delete-btn">Delete</button>
        `;

        const checkbox = li.querySelector("input");
        const deleteBtn = li.querySelector(".delete-btn");

        checkbox.addEventListener("change", () => {
            task.completed = checkbox.checked;
            saveTasks();
            renderTasks();
        });

        deleteBtn.addEventListener("click", () => {
            tasks = tasks.filter(item => item.id !== task.id);
            saveTasks();
            renderTasks();
        });

        taskList.appendChild(li);
    });

    const pendingTasks =
        tasks.filter(task => !task.completed).length;

    taskCount.textContent =
        `${pendingTasks} ${pendingTasks === 1 ? "task" : "tasks"} remaining`;
}

function addTask() {
    const text = taskInput.value.trim();

    if (text === "") {
        alert("Please enter a task.");
        return;
    }

    tasks.push({
        id: Date.now(),
        text: text,
        completed: false
    });

    taskInput.value = "";
    saveTasks();
    renderTasks();
    taskInput.focus();
}

function escapeHTML(text) {
    const div = document.createElement("div");
    div.textContent = text;
    return div.innerHTML;
}

addTaskBtn.addEventListener("click", addTask);

taskInput.addEventListener("keydown", (event) => {
    if (event.key === "Enter") {
        addTask();
    }
});

clearCompleted.addEventListener("click", () => {
    tasks = tasks.filter(task => !task.completed);
    saveTasks();
    renderTasks();
});

renderTasks();
```

## OUTPUT

<img width="1915" height="969" alt="Screenshot 2026-08-17 112618" src="https://github.com/user-attachments/assets/1c64d10d-dfcc-4194-8762-0d9685566dcc" />


## RESULT

The program for creating To-do list using JavaScript is executed successfully.
