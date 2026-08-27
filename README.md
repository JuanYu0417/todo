# Todo App
A simple Todo application built with React and Vite.

## Features

- Add a new task
- Display tasks in a list
- Delete tasks

## Technologies
React
Vite
JavaScript
CSS
ESLint

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
```json
"scripts": {
"test": "echo \"Error: no test specified\" && exit 1",
"dev": "nodemon index.js"
}
```
### Create Express server
```json
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

```json
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
```json
"scripts": {
  "test": "mocha *.test.js",
  "dev": "nodemon index.js"
}
```
### create test.js under/server,then rename index.test.js

```json
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
