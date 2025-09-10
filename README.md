# Ex03 To-Do List using JavaScript
## Date:08-09-2025
## Reg no: 212223220107
## Name: Sonu S

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
Create a To-do application using Javascript.

### STEP 5
Add functionalities.

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
## Index.html
```
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>To-Do App with Filters</title>
  <link rel="stylesheet" href="styles.css">
</head>
<body>
  <div class="app">
    <h1>My To-Do List</h1>

    <!-- Input -->
    <div class="todo-input">
      <input type="text" id="taskInput" placeholder="Add a new task...">
      <button id="addBtn">Add</button>
    </div>

    <!-- Filter buttons -->
    <div class="filters">
      <button data-filter="all" class="filter-btn active">All</button>
      <button data-filter="completed" class="filter-btn">Completed</button>
      <button data-filter="not-completed" class="filter-btn">Not Completed</button>
    </div>

    <!-- Task list -->
    <ul id="taskList"></ul>
  </div>

  <script src="script.js"></script>
</body>
</html>

```

## Styles.css
```
body {
  font-family: Arial, sans-serif;
  background: #f0f2f5;
  display: flex;
  justify-content: center;
  align-items: center;
  min-height: 100vh;
  margin: 0;
}

.app {
  background: #fff;
  padding: 20px;
  border-radius: 12px;
  box-shadow: 0px 4px 10px rgba(0,0,0,0.1);
  width: 350px;
}

h1 {
  text-align: center;
  color: #333;
}

.todo-input {
  display: flex;
  margin-bottom: 15px;
}

#taskInput {
  flex: 1;
  padding: 10px;
  border: 2px solid #ddd;
  border-radius: 8px;
}

#addBtn {
  margin-left: 10px;
  padding: 10px 15px;
  border: none;
  background: #007bff;
  color: white;
  border-radius: 8px;
  cursor: pointer;
}

.filters {
  text-align: center;
  margin-bottom: 15px;
}

.filter-btn {
  margin: 0 5px;
  padding: 5px 10px;
  border: none;
  border-radius: 6px;
  background: #ddd;
  cursor: pointer;
}

.filter-btn.active {
  background: #001a17;
  color: white;
}

#taskList {
  list-style: none;
  padding: 0;
}

#taskList li {
  display: flex;
  justify-content: space-between;
  align-items: center;
  background: #f8f9fa;
  margin-bottom: 8px;
  padding: 8px;
  border-radius: 8px;
}

.task-left {
  display: flex;
  align-items: center;
}

.task-text {
  margin-left: 8px;
}

.completed .task-text {
  text-decoration: line-through;
  color: gray;
}

.delete-btn {
  background: #dc3545;
  color: white;
  border: none;
  padding: 5px 8px;
  border-radius: 6px;
  cursor: pointer;
}

.edit-btn {
  background: #89a9e1;
  color: black;
  border: none;
  padding: 5px 8px;
  border-radius: 6px;
  cursor: pointer;
  margin-right: 5px;
}

.edit-btn:hover {
  background: #27ced4;
}

```

## Script.js
```
const taskInput = document.getElementById("taskInput");
const addBtn = document.getElementById("addBtn");
const taskList = document.getElementById("taskList");
const filterBtns = document.querySelectorAll(".filter-btn");

// Load tasks from localStorage
window.onload = () => {
  const savedTasks = JSON.parse(localStorage.getItem("tasks")) || [];
  savedTasks.forEach(task => addTask(task.text, task.completed));
};

// Add task
function addTask(taskText, completed = false) {
  if (taskText.trim() === "") return;

  const li = document.createElement("li");
  if (completed) li.classList.add("completed");

  // Left side: checkbox + text
  const leftDiv = document.createElement("div");
  leftDiv.classList.add("task-left");

  const checkbox = document.createElement("input");
  checkbox.type = "checkbox";
  checkbox.checked = completed;

  const span = document.createElement("span");
  span.textContent = taskText;
  span.classList.add("task-text");

  checkbox.addEventListener("change", () => {
    li.classList.toggle("completed", checkbox.checked);
    saveTasks();
    applyFilter();
  });

  leftDiv.appendChild(checkbox);
  leftDiv.appendChild(span);

  // --- Buttons container ---
  const btnGroup = document.createElement("div");

  // Edit button
  const editBtn = document.createElement("button");
  editBtn.textContent = "Edit";
  editBtn.classList.add("edit-btn");
  editBtn.addEventListener("click", () => {
    editTask(li, span, editBtn);
  });

  // Delete button
  const deleteBtn = document.createElement("button");
  deleteBtn.textContent = "Delete";
  deleteBtn.classList.add("delete-btn");
  deleteBtn.addEventListener("click", () => {
    li.remove();
    saveTasks();
  });

  btnGroup.appendChild(editBtn);
  btnGroup.appendChild(deleteBtn);

  li.appendChild(leftDiv);
  li.appendChild(btnGroup);
  taskList.appendChild(li);

  saveTasks();
  applyFilter();
}

// Add button event
addBtn.addEventListener("click", () => {
  addTask(taskInput.value);
  taskInput.value = "";
});

// Enter key to add task
taskInput.addEventListener("keypress", (e) => {
  if (e.key === "Enter") {
    addTask(taskInput.value);
    taskInput.value = "";
  }
});

// Save to localStorage
function saveTasks() {
  const tasks = [];
  document.querySelectorAll("#taskList li").forEach(li => {
    tasks.push({
      text: li.querySelector(".task-text").textContent,
      completed: li.classList.contains("completed")
    });
  });
  localStorage.setItem("tasks", JSON.stringify(tasks));
}

// Filter tasks
filterBtns.forEach(btn => {
  btn.addEventListener("click", () => {
    filterBtns.forEach(b => b.classList.remove("active"));
    btn.classList.add("active");
    applyFilter();
  });
});

function applyFilter() {
  const activeFilter = document.querySelector(".filter-btn.active").dataset.filter;
  document.querySelectorAll("#taskList li").forEach(li => {
    switch (activeFilter) {
      case "all":
        li.style.display = "flex";
        break;
      case "completed":
        li.style.display = li.classList.contains("completed") ? "flex" : "none";
        break;
      case "not-completed":
        li.style.display = !li.classList.contains("completed") ? "flex" : "none";
        break;
    }
  });
}

// Edit task
function editTask(li, span, editBtn) {
  const currentText = span.textContent;

  // Create input field with current text
  const input = document.createElement("input");
  input.type = "text";
  input.value = currentText;
  input.classList.add("edit-input");

  // Create save button
  const saveBtn = document.createElement("button");
  saveBtn.textContent = "Save";
  saveBtn.classList.add("edit-btn");

  // Replace span with input
  li.querySelector(".task-left").replaceChild(input, span);

  // Replace Edit button with Save button
  editBtn.replaceWith(saveBtn);

  // Save edited text
  saveBtn.addEventListener("click", () => {
    const newText = input.value.trim();
    if (newText !== "") {
      span.textContent = newText;
      li.querySelector(".task-left").replaceChild(span, input);
      saveBtn.replaceWith(editBtn); // put Edit button back
      saveTasks();
    }
  });
}

```


## OUTPUT

<img width="1919" height="935" alt="Screenshot 2025-09-10 092315" src="https://github.com/user-attachments/assets/47b36923-70ae-4c9d-8932-5cfef149cb8a" />

## RESULT
The program for creating To-do list using JavaScript is executed successfully.
