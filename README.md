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
## Environment variables, routing and backend middleware
### env
By default, Windows uses port **5432** to access Postgres and Mac port **5435**.
Install dotenv library
```bash
npm i dotenv --save
```
Install cross-env library
```bash
npm i cross-env --save-dev
```
modify scripts in package.json
```js
"scripts": {
  "test": "cross-env NODE_ENV=test mocha *.test.js",
  "dev": "cross-env NODE_ENV=development nodemon index.js"
}
```
and database in index.js
Update .gitignore
Create another PostgreSQL database named test_todo by pgAdmin 4.
add a test script in package.json
```js
"start:test": "cross-env NODE_ENV=test node index.js"
```
### Separation of Concerns (SoC): One responsibility per file, like as :  
| Responsibility | File |
|---------------|------|
| Environment variables configuration | config.js |
| Database connection | db.js |
| API routes | routes/tasks.js |
| Middleware | middleware/*.js |
| Server startup | index.js |

### Create server/helper/db.js and move the PostgreSQL pool configuration there
```js
import { pool } from './helper/db.js'
```
### Route
Move GET,POST,DELETE endpoints from index.js to a todoRouter.js.
```js
const router = Router()
```
### add error middleware after router
```js
app.use((err, req, res, next)

return next(err)
```
## Authentication and authorization on backend
Authentication verifies who the user is, while Authorization determines what the user is allowed to do.
###  Install jsonwebtoken and bcrypt.
```bash
npm i jsonwebtoken bcrypt
```
### Add an account table to the database.
```sql
drop table if exists account;

create table account (
    id serial primary key,
    email varchar(50) not null unique,
    password varchar(255) not null
);
```
### Add userRouter.js under /routes 
```js
import { Router } from 'express'
import { compare, hash } from 'bcrypt'
import jwt from 'jsonwebtoken'
import { pool } from '../helper/db.js'

const { sign } = jwt
const router = Router()
router.post('/signup', async (req, res, next) => {})
```
### Mount userRouter in server/index.js at /users.
```js
import userRouter from "./routes/userRouter.js"
app.use('/users',userRouter)
```
### Add user management test in index.test.js
```js
describe("Testing user management", () => {}
```
Then test
### Initialize database to make the test repeatable
create test.js under /helper
```js
const initializeTestDb = async ()
```
import function to index.test.js
```js
import { initializeTestDb } from './helper/test.js'

beforeEach(async () => {
  await initializeTestDb()
})
```
### Add JWT_SECRET_KEY to server/.env 
```js
JWT_SECRET_KEY=
```
### Add signin endpoint to userRouter.js
```js
router.post('/signin', async (req, res, next) 
```
### Create insertTestUser() in helper/test.js.
```js
import { hash } from 'bcrypt'
const insertTestUser = async (user) => {}
export { initializeTestDb, insertTestUser }
```
### Create a test-user object in index.test.js
```js
import {initializeTestDb,insertTestUser} from './helper/test.js'

describe("Testing user management", () => {
  const user = { email: "foo2@test.com", password: "password123" }
  before(async () => {
  await insertTestUser(user)
})
```
### create POST /users/signin in index.test.js
```js
it('should log in', async () => {}
```
### Create helper/auth.js and add auth to todoRouter.js
```js
import jwt from 'jsonwebtoken'
const { verify } = jwt
const auth = (req, _res, next) => {}
export { auth }
```
in todoRouter.js
```js
import { auth } from '../helper/auth.js'
router.post('/', auth, (req, res,next) => { }
router.delete('/tasks/:id', auth, (req, res,next) => {}
```
### import jwt and add getToken to test.js
```js
import jwt from 'jsonwebtoken'
const getToken = (email) =>{}
```
### test
```bash
nom run start:test
npm test
```
## Authentication and authorization on the frontend
### Install react-touter-dom
```bash
npm install react-router-dom
```
**react-router-dom** is a React library used to create multiple pages and navigation in a React web application.
### Create Authentication.jsx and NotFound.jsx
```jsx
export const AuthenticationMode = object.freeze({})
export default function Authentication({authenticationMode }) {}
```
```jsx
export default function NotFound() {}
```
### edit main.jsx
```jsx
import Authentication, { AuthenticationMode } from './screens/Authentication'
import ProtectedRoute from './components/ProtectedRoute'
import UserProvider from './context/UserProvider'
import { RouterProvider } from 'react-router-dom'
import { createBrowserRouter } from "react-router-dom";
import NotFound from "./screens/NotFound";

const router = createBrowserRouter([])
createRoot(document.getElementById('root')).render()
```
### Create folder context under /src  and three files
install dotenv /todo
```jsx
npm i dotenv --save
```
then create .env /todo
```env
VITE_API_URL=http://localhost:3001
```
**UserContext.js** Creates the React Context used to share user data across components.  
**UserProvider.js** Provides the user state and functions through the Context Provider.  
**useUser.js** Custom hook that allows components to easily access the User Context.  
Files:
- ./src/context/UserContext.js
- ./src/context/UserProvider.js
- ./src/context/useUser.js
### /components define ProtectedRoute.jsx
```jsx
export default function ProtectedRoute() {}
```
### import useUser to Authentication.jsx
```jsx
import { Link,useNavigate } from "react-router-dom"
import { useUser } from "../context/useUser"
export default function Authentication({authenticationMode}) {}
```
### import useUser to App.jsx
```jsx
import { useUser } from './context/useUser'

const { user } = useUser()
const headers = {headers: {Authorization: `Bearer ${user.token}`}}
```