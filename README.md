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

## Installation

### 1. Run the app

```bash
npm install
npm run dev
```
Open the browser and navigate to:
http://localhost:5173

### 2.create backend

2.1 create server

mkdir server
cd server
npm init -y
```

2.2 create PostgreSQL
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

2.3 install independencies /server
```bash
npm install express cors pg
npm install --save-dev nodemon
```

2.4 Configure package.json
Add a development script to `server/package.json`:
```json
"scripts": {
"test": "echo \"Error: no test specified\" && exit 1",
"dev": "nodemon index.js"
}
```
2.5 Create Express server
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

2.6 Create API endpoint

```json
app.get('/tasks', ...)
app.post('/tasks', ...)
app.delete('/tasks/:id', ...)
```