# Task Management API

A RESTful API built with Node.js, Express.js, and MongoDB for managing tasks with full CRUD operations, pagination, and sorting capabilities.

## 📋 Table of Contents
- [Features](#features)
- [Tech Stack](#tech-stack)
- [Prerequisites](#prerequisites)
- [Installation](#installation)
- [Configuration](#configuration)
- [Running the Application](#running-the-application)
- [API Endpoints](#api-endpoints)
- [Testing with Postman](#testing-with-postman)
- [Project Structure](#project-structure)
- [Error Handling](#error-handling)
- [Bonus Features](#bonus-features)

---

## ✨ Features

- ✅ Create, Read, Update, and Delete tasks (CRUD operations)
- ✅ Input validation for all fields
- ✅ Comprehensive error handling
- ✅ Pagination support for large datasets
- ✅ Sorting by creation date (newest first)
- ✅ RESTful API design
- ✅ MongoDB integration with Mongoose ODM

---

## 🛠️ Tech Stack

- **Runtime:** Node.js with Bun
- **Framework:** Express.js
- **Database:** MongoDB (MongoDB Atlas)
- **ODM:** Mongoose
- **Environment Variables:** dotenv
- **CORS:** cors middleware

---

## 📦 Prerequisites

Before you begin, ensure you have the following installed:

- **Bun** (v1.0 or higher) - [Install Bun](https://bun.sh/docs/installation)
- **MongoDB Atlas Account** (Free tier) - [Sign up here](https://www.mongodb.com/cloud/atlas)
- **Postman** (for API testing) - [Download Postman](https://www.postman.com/downloads/)
- **Git** (optional, for version control)

---

## 🚀 Installation

### Step 1: Clone or Create Project Directory

```bash
mkdir task-management-api
cd task-management-api
```

### Step 2: Initialize Project

```bash
bun init -y
```

### Step 3: Install Dependencies

```bash
bun add express mongoose dotenv cors
```

### Step 4: Create Project Structure

```bash
mkdir models routes
```

Your folder structure should look like this:

```
task-management-api/
├── models/
│   └── Task.js
├── routes/
│   └── tasks.js
├── .env
├── .gitignore
├── package.json
├── README.md
└── server.js
```

### Step 5: Create Files

Create the following files with their respective content:

#### **`.gitignore`**
```
node_modules/
.env
bun.lockb
.DS_Store
```

#### **`server.js`**
```javascript
// Copy from provided artifact: server.js
```

#### **`models/Task.js`**
```javascript
// Copy from provided artifact: models/Task.js - CORRECTED Task Model
```

#### **`routes/tasks.js`**
```javascript
// Copy from provided artifact: routes/tasks.js - WITH PAGINATION & SORTING
```

---

## ⚙️ Configuration

### Step 1: Setup MongoDB Atlas

1. Go to [MongoDB Atlas](https://www.mongodb.com/cloud/atlas)
2. Create a free account (if you don't have one)
3. Create a new cluster (free M0 tier)
4. Click **"Connect"** on your cluster
5. Choose **"Connect your application"**
6. Select **"Drivers"** as the connection method
7. Copy your connection string

### Step 2: Create `.env` File

Create a `.env` file in the root directory:

```env
PORT=5000
MONGODB_URI=mongodb+srv://YOUR_USERNAME:YOUR_PASSWORD@YOUR_CLUSTER.mongodb.net/taskmanager?retryWrites=true&w=majority&appName=YOUR_APP_NAME
```

**Important:** 
- Replace `YOUR_USERNAME` with your MongoDB username
- Replace `YOUR_PASSWORD` with your MongoDB password
- Replace `YOUR_CLUSTER` with your cluster address
- Replace `YOUR_APP_NAME` with your app name
- Keep `taskmanager` as the database name (it will be auto-created)

**Example:**
```env
PORT=5000
MONGODB_URI=mongodb+srv://john:myPassword123@cluster0.abcde.mongodb.net/taskmanager?retryWrites=true&w=majority&appName=taskapp
```

### Step 3: Configure MongoDB Atlas Network Access

1. In MongoDB Atlas, go to **Network Access**
2. Click **"Add IP Address"**
3. Click **"Allow Access from Anywhere"** (for development only)
4. Click **"Confirm"**

---

## 🏃 Running the Application

### Development Mode (with auto-restart)

```bash
bun run dev
```

### Production Mode

```bash
bun start
```

### Expected Output

```
✅ Connected to MongoDB
🚀 Server running on http://localhost:5000
```

### Verify Server is Running

Open your browser and go to:
```
http://localhost:5000
```

You should see:
```json
{
  "message": "Welcome to Task Management API",
  "endpoints": {
    "GET /api/tasks": "Get all tasks",
    "GET /api/tasks/:id": "Get task by ID",
    "POST /api/tasks": "Create new task",
    "PUT /api/tasks/:id": "Update task",
    "DELETE /api/tasks/:id": "Delete task"
  }
}
```

---

## 📚 API Endpoints

### Base URL
```
http://localhost:5000
```

### Endpoints Overview

| Method | Endpoint | Description | Requires Body |
|--------|----------|-------------|---------------|
| GET | `/` | API information | No |
| GET | `/api/tasks` | Get all tasks (with pagination) | No |
| GET | `/api/tasks/:id` | Get single task by ID | No |
| POST | `/api/tasks` | Create new task | Yes |
| PUT | `/api/tasks/:id` | Update task | Yes |
| DELETE | `/api/tasks/:id` | Delete task | No |

---

### Detailed Endpoint Documentation

#### 1. **GET /** - API Information
Get basic API information and available endpoints.

**Request:**
```
GET http://localhost:5000/
```

**Response:**
```json
{
  "message": "Welcome to Task Management API",
  "endpoints": { ... }
}
```

---

#### 2. **POST /api/tasks** - Create New Task

**Request:**
```
POST http://localhost:5000/api/tasks
Content-Type: application/json
```

**Body:**
```json
{
  "title": "Complete API Project",
  "description": "Build and test the task management API",
  "status": "in-progress"
}
```

**Fields:**
- `title` (String, Required) - Task title
- `description` (String, Optional) - Task description
- `status` (String, Optional) - Task status: `pending`, `in-progress`, or `completed` (Default: `pending`)

**Response (201 Created):**
```json
{
  "success": true,
  "message": "Task created successfully",
  "data": {
    "_id": "671234567890abcdef123456",
    "title": "Complete API Project",
    "description": "Build and test the task management API",
    "status": "in-progress",
    "createdAt": "2024-10-18T12:30:45.123Z"
  }
}
```

---

#### 3. **GET /api/tasks** - Get All Tasks

**Request (Without Pagination):**
```
GET http://localhost:5000/api/tasks
```

**Request (With Pagination):**
```
GET http://localhost:5000/api/tasks?page=1&limit=5
```

**Query Parameters:**
- `page` (Number, Optional) - Page number (Default: 1)
- `limit` (Number, Optional) - Items per page (Default: 10)

**Response:**
```json
{
  "success": true,
  "count": 5,
  "pagination": {
    "currentPage": 1,
    "totalPages": 3,
    "totalTasks": 15,
    "tasksPerPage": 5,
    "hasNextPage": true,
    "hasPrevPage": false
  },
  "data": [
    {
      "_id": "671234567890abcdef123456",
      "title": "Complete API Project",
      "description": "Build and test the task management API",
      "status": "in-progress",
      "createdAt": "2024-10-18T12:30:45.123Z"
    },
    // ... more tasks
  ]
}
```

**Note:** Tasks are automatically sorted by `createdAt` in descending order (newest first).

---

#### 4. **GET /api/tasks/:id** - Get Single Task

**Request:**
```
GET http://localhost:5000/api/tasks/671234567890abcdef123456
```

**Response (200 OK):**
```json
{
  "success": true,
  "data": {
    "_id": "671234567890abcdef123456",
    "title": "Complete API Project",
    "description": "Build and test the task management API",
    "status": "in-progress",
    "createdAt": "2024-10-18T12:30:45.123Z"
  }
}
```

**Response (404 Not Found):**
```json
{
  "success": false,
  "message": "Task not found"
}
```

---

#### 5. **PUT /api/tasks/:id** - Update Task

**Request:**
```
PUT http://localhost:5000/api/tasks/671234567890abcdef123456
Content-Type: application/json
```

**Body (Update any field):**
```json
{
  "status": "completed"
}
```

**Or update multiple fields:**
```json
{
  "title": "Updated Title",
  "description": "Updated description",
  "status": "completed"
}
```

**Response (200 OK):**
```json
{
  "success": true,
  "message": "Task updated successfully",
  "data": {
    "_id": "671234567890abcdef123456",
    "title": "Complete API Project",
    "description": "Build and test the task management API",
    "status": "completed",
    "createdAt": "2024-10-18T12:30:45.123Z"
  }
}
```

---

#### 6. **DELETE /api/tasks/:id** - Delete Task

**Request:**
```
DELETE http://localhost:5000/api/tasks/671234567890abcdef123456
```

**Response (200 OK):**
```json
{
  "success": true,
  "message": "Task deleted successfully",
  "data": {
    "_id": "671234567890abcdef123456",
    "title": "Complete API Project",
    "description": "Build and test the task management API",
    "status": "completed",
    "createdAt": "2024-10-18T12:30:45.123Z"
  }
}
```

---

## 🧪 Testing with Postman

### Setting Up Postman

1. Open Postman
2. Create a new Collection named **"Task Management API"**
3. Set the base URL variable (optional):
   - Click on Collection → Variables
   - Add variable: `baseUrl` = `http://localhost:5000`

### Test Sequence

Follow this sequence for comprehensive testing:

#### **Test 1: Create Multiple Tasks**

Create at least 5-7 tasks to properly test pagination.

**Request:**
- Method: POST
- URL: `http://localhost:5000/api/tasks`
- Body (raw → JSON):
```json
{
  "title": "Learn Node.js",
  "description": "Complete Node.js tutorial",
  "status": "in-progress"
}
```

**Save the `_id` from the first task response for later tests!**

#### **Test 2: Get All Tasks (No Pagination)**

**Request:**
- Method: GET
- URL: `http://localhost:5000/api/tasks`

#### **Test 3: Get Tasks with Pagination**

**Request:**
- Method: GET
- URL: `http://localhost:5000/api/tasks?page=1&limit=3`

Try different values:
- `?page=1&limit=5`
- `?page=2&limit=5`
- `?page=1&limit=2`

#### **Test 4: Get Single Task**

**Request:**
- Method: GET
- URL: `http://localhost:5000/api/tasks/YOUR_TASK_ID`

Replace `YOUR_TASK_ID` with actual task ID from Test 1.

#### **Test 5: Update Task**

**Request:**
- Method: PUT
- URL: `http://localhost:5000/api/tasks/YOUR_TASK_ID`
- Body:
```json
{
  "status": "completed"
}
```

#### **Test 6: Delete Task**

**Request:**
- Method: DELETE
- URL: `http://localhost:5000/api/tasks/YOUR_TASK_ID`

#### **Test 7: Verify Deletion**

**Request:**
- Method: GET
- URL: `http://localhost:5000/api/tasks`

The deleted task should not appear in the list.

### Error Testing

#### **Test 8: Missing Required Field**

**Request:**
- Method: POST
- URL: `http://localhost:5000/api/tasks`
- Body:
```json
{
  "description": "No title provided"
}
```

**Expected:** 400 Bad Request with validation error

#### **Test 9: Invalid Status Value**

**Request:**
- Method: POST
- URL: `http://localhost:5000/api/tasks`
- Body:
```json
{
  "title": "Test Task",
  "status": "invalid-status"
}
```

**Expected:** 400 Bad Request with validation error

#### **Test 10: Invalid Task ID Format**

**Request:**
- Method: GET
- URL: `http://localhost:5000/api/tasks/123`

**Expected:** 500 Internal Server Error with "Cast to ObjectId failed" message

#### **Test 11: Non-existent Task ID**

**Request:**
- Method: GET
- URL: `http://localhost:5000/api/tasks/671234567890abcdef999999`

**Expected:** 404 Not Found with "Task not found" message

### Taking Screenshots

For each test, capture a screenshot showing:
1. ✅ The complete URL
2. ✅ The HTTP method (GET, POST, PUT, DELETE)
3. ✅ The request body (if applicable)
4. ✅ The response status code (200, 201, 404, etc.)
5. ✅ The complete response body

### Exporting Postman Collection

1. Right-click on your collection
2. Select **"Export"**
3. Choose **"Collection v2.1"**
4. Save the JSON file
5. Include this file in your submission

---

## 📁 Project Structure

```
task-management-api/
├── models/
│   └── Task.js              # Task schema and model definition
├── routes/
│   └── tasks.js             # All API route handlers
├── .env                     # Environment variables (not in git)
├── .gitignore              # Git ignore file
├── package.json            # Project dependencies and scripts
├── README.md               # Project documentation (this file)
└── server.js               # Main application entry point
```

### File Descriptions

- **`server.js`**: Main application file that sets up Express server, connects to MongoDB, and configures middleware
- **`models/Task.js`**: Mongoose schema defining the Task model structure and validation rules
- **`routes/tasks.js`**: Contains all route handlers for CRUD operations with pagination and error handling
- **`.env`**: Environment configuration file containing sensitive data (database URI, port)
- **`package.json`**: Node.js project configuration with dependencies and scripts

---

## 🔒 Task Schema

```javascript
{
  title: {
    type: String,
    required: true,
    trim: true
  },
  description: {
    type: String,
    trim: true,
    default: ''
  },
  status: {
    type: String,
    enum: ['pending', 'in-progress', 'completed'],
    default: 'pending'
  },
  createdAt: {
    type: Date,
    default: Date.now
  }
}
```

### Field Descriptions

| Field | Type | Required | Default | Validation |
|-------|------|----------|---------|------------|
| `title` | String | Yes | - | Must not be empty |
| `description` | String | No | `''` | - |
| `status` | String | No | `pending` | Must be: `pending`, `in-progress`, or `completed` |
| `createdAt` | Date | No | Current date/time | Auto-generated |

---

## ⚠️ Error Handling

The API implements comprehensive error handling for common scenarios:

### Validation Errors (400 Bad Request)

**Missing Required Fields:**
```json
{
  "success": false,
  "message": "Error creating task",
  "error": "Task validation failed: title: Task title is required"
}
```

**Invalid Status Value:**
```json
{
  "success": false,
  "message": "Error creating task",
  "error": "Task validation failed: status: `invalid-status` is not a valid enum value"
}
```

### Not Found Errors (404 Not Found)

**Task Not Found:**
```json
{
  "success": false,
  "message": "Task not found"
}
```

### Server Errors (500 Internal Server Error)

**Invalid ObjectId:**
```json
{
  "success": false,
  "message": "Error fetching task",
  "error": "Cast to ObjectId failed for value \"123\""
}
```

**Database Connection Error:**
```json
{
  "success": false,
  "message": "Error fetching tasks",
  "error": "Connection refused"
}
```

---

## 🎁 Bonus Features

### 1. Pagination

Implemented using query parameters to handle large datasets efficiently.

**Usage:**
```
GET /api/tasks?page=1&limit=10
```

**Parameters:**
- `page`: Page number (default: 1)
- `limit`: Items per page (default: 10)

**Response includes pagination metadata:**
```json
{
  "pagination": {
    "currentPage": 1,
    "totalPages": 5,
    "totalTasks": 50,
    "tasksPerPage": 10,
    "hasNextPage": true,
    "hasPrevPage": false
  }
}
```

### 2. Sorting by Creation Date

All GET requests automatically sort tasks by `createdAt` in descending order (newest first).

**Implementation:**
```javascript
.sort({ createdAt: -1 })  // -1 = descending (newest first)
```

---

## 🐛 Troubleshooting

### Common Issues and Solutions

#### Issue: "connect ECONNREFUSED"

**Cause:** MongoDB connection failed  
**Solution:**
1. Check your MongoDB Atlas cluster is running
2. Verify your connection string in `.env`
3. Check network access settings in MongoDB Atlas
4. Ensure your IP is whitelisted

#### Issue: "Port 5000 already in use"

**Cause:** Another process is using port 5000  
**Solution:**
1. Change PORT in `.env` to another port (e.g., 3000, 8080)
2. Or kill the process using port 5000

#### Issue: "Cannot find module 'express'"

**Cause:** Dependencies not installed  
**Solution:**
```bash
bun install
```

#### Issue: "Task validation failed"

**Cause:** Invalid data sent to API  
**Solution:**
1. Check required fields are included
2. Verify status values are: `pending`, `in-progress`, or `completed`
3. Ensure JSON format is correct

#### Issue: "Cast to ObjectId failed"

**Cause:** Invalid MongoDB ObjectId format  
**Solution:**
1. Use a valid 24-character hexadecimal task ID
2. Get valid IDs from GET /api/tasks response

---

## 👨‍💻 Author

**Ruchir Kalokhe**  
Sourceagle Internship - October 2025

---

## 🎯 API Testing Summary

**Total Endpoints:** 6  
**CRUD Operations:** 5  
**Bonus Features:** 2 (Pagination + Sorting)  
**Error Scenarios Handled:** 4+

**Recommended Test Count:** 13 tests minimum  
**Required Screenshots:** 13+ screenshots

---

**Last Updated:** October 18, 2025  1:11 p.m.. IST
