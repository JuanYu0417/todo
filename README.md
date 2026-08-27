# Todo App
A simple Todo application built with React and Vite.

## Features

- Add a new task
- Display tasks in a list
- Delete tasks
- Fetch tasks from a PostgreSQL database
- REST API communication using Axios

## Technologies
React
Vite
JavaScript
CSS
ESLint
PostgreSQL
Mocha
Chai
Axois

## Run the app

```bash
npm install
npm run dev
```
Open the browser and navigate to:
http://localhost:5173

## create backend

### create server
```bash
mkdir server
cd server
npm init -y
```

###  create PostgreSQL

download: https://www.postgresql.org/download/
after install, add path to env variable,then test:
```bash
psql --version
psql -U postgres
CREATE DATABASE todo;
```
Then create database.qsl under /server
```bash
\c todo
\q
psql -U postgres -d todo -f database.sql
```
or using pgAdmin 4 to manage postgreSQL

### install independencies /server
```bash
npm install express cors pg
npm install --save-dev nodemon
```

### Configure package.json
Add a development script to `server/package.json`:
```js
"scripts": {
"test": "echo \"Error: no test specified\" && exit 1",
"dev": "nodemon index.js"
}
```
### Create Express server
```js
import express from 'express'
import cors from 'cors'

const app = express()
const port = 3001

app.use(cors())
app.use(express.json())

app.get('/tasks', ...)

app.listen(3001, ...)
```

Verify the server is running:
http://localhost:3001

### Create API endpoint

```js
app.get('/tasks', ...)
app.post('/tasks', ...)
app.delete('/tasks/:id', ...)
```

##  Automated testing
- Creating tests with the Mocha and Chai libraries for a Node.js/Express application
- Test a REST API
### Install Mocha and Chai
```bash
npm i mocha chai --save-dev
```
### modify script to server/package.json
```js
"scripts": {
  "test": "mocha *.test.js",
  "dev": "nodemon index.js"
}
```
### create test.js under/server,then rename index.test.js

```js
describe("Testing basic database functionality", () => {
  it("should get all tasks", ...)
  it("should create a new task", ...)
  it("should delete task", ...)
  it("should not create a new task without description", ...)
})
```

### run test
```bash
npm run test
```
## Calling the REST API from React
### Quick Notes 
- async/await: Handles asynchronous operations in a readable way. 
- Axios: Sends HTTP requests to APIs. 
- useEffect(..., []): Executes code once when the component mounts, often used for data fetching.

### install axios /todo
```bash
npm install axios
```
### Import axios in /src/App.jsx and add useEffect to the React import.
```js
import {useState,useEffect} from 'react'
import axios from 'axios'
```
### Frontend Data Fetching /src/App.jsx
- Axios is used to send HTTP requests to the backend API.
- apiUrl stores the base URL of the backend server.
- useEffect(..., []) runs once when the component is mounted.
- axios.get() fetches tasks from the backend.
- catch() handles request errors and displays an alert message.

### Update Task Rendering
- Use `item.id` as the React key.
- Display `item.description` instead of `item`.
Before:
["Task 1", "Task 2"]
After:
{ id: 1, description: "Task 1" },
{ id: 2, description: "Task 2" }

### Add task function
```js
const addTask = (event)
const deleteTask = (deleted)
```
###  Create a Row component Row.jsx in src/components.
```js
export default function Row()
```
import row to tasks.map in App.jsx
```js
import Row from './components/Row.jsx'

tasks.map(task => (
<Row task={task} key={task.id} onDelete={deleteTask} />
))
```