# Node.js and Express.js Learning Notes

## Table of Contents

1. [Asynchronous Programming](#asynchronous-programming)
   - [Callbacks](#callbacks)
   - [Promises](#promises)
   - [Async/Await](#asyncawait)
   - [Exercises](#exercises-1)
2. [Node.js Architecture](#nodejs-architecture)
3. [Core Modules](#core-modules)
   - [HTTP](#http)
   - [File System (fs)](#file-system-fs)
   - [Path](#path)
   - [Events](#events)
   - [Exercises](#exercises-2)
4. [Global Objects](#global-objects)
   - [Process](#process)
   - [__dirname and __filename](#__dirname-and-__filename)
   - [Command-line Arguments](#command-line-arguments)
   - [Exercises](#exercises-3)
5. [Express.js](#expressjs)
   - [Request and Response Objects](#request-and-response-objects)
   - [Routing](#routing)
   - [Middleware](#middleware)
   - [Serving Static Files](#serving-static-files)
   - [Exercises](#exercises-4)
6. [Mongoose](#mongoose)
   - [Schema Design and Validation](#schema-design-and-validation)
   - [User Authentication](#user-authentication)
   - [Role-Based Access Control (RBAC)](#role-based-access-control-rbac)
   - [Exercises](#exercises-5)
7. [Event Loop and Asynchronous Operations](#event-loop-and-asynchronous-operations)
   - [Exercises](#exercises-6)
8. [Streams](#streams)
   - [Exercises](#exercises-7)
9. [Clustering and Worker Threads](#clustering-and-worker-threads)
   - [Exercises](#exercises-8)
10. [Unit Testing](#unit-testing)
   - [Exercises](#exercises-9)
11. [Integration and End-to-End Testing](#integration-and-end-to-end-testing)
   - [Exercises](#exercises-10)
12. [Common Security Threats](#common-security-threats)
   - [Exercises](#exercises-11)
13. [Secure Authentication](#secure-authentication)
   - [Exercises](#exercises-12)
14. [Profiling and Monitoring](#profiling-and-monitoring)
   - [Exercises](#exercises-13)

---

## Asynchronous Programming

### Callbacks

Callbacks are functions passed as arguments to other functions and are executed after some operation has been completed.

```javascript
function fetchData(callback) {
  setTimeout(() => {
    callback('Data fetched');
  }, 2000);
}

fetchData((data) => {
  console.log(data); // Data fetched
});
```

### Promises

Promises represent a value that may be available now, in the future, or never.

```javascript
const fetchData = new Promise((resolve, reject) => {
  setTimeout(() => {
    resolve('Data fetched');
  }, 2000);
});

fetchData.then(data => {
  console.log(data); // Data fetched
});
```

### Async/Await

Async/Await allows you to write asynchronous code in a synchronous manner.

```javascript
function fetchData() {
  return new Promise((resolve) => {
    setTimeout(() => {
      resolve('Data fetched');
    }, 2000);
  });
}

async function getData() {
  const data = await fetchData();
  console.log(data); // Data fetched
}

getData();
```

### Exercises

#### Exercise 1: Create a Simple Promise

Create a simple promise that resolves after 2 seconds.

```javascript
const simplePromise = new Promise((resolve) => {
  setTimeout(() => {
    resolve('Promise resolved after 2 seconds');
  }, 2000);
});

simplePromise.then(message => console.log(message));
```

#### Exercise 2: Async Function

Write an async function that waits for the promise to resolve and then logs a message.

```javascript
async function asyncFunction() {
  const message = await simplePromise;
  console.log(message); // Promise resolved after 2 seconds
}

asyncFunction();
```

---

## Node.js Architecture

Node.js is a JavaScript runtime built on Chrome's V8 JavaScript engine. It uses an event-driven, non-blocking I/O model that makes it lightweight and efficient.

---

## Core Modules

### HTTP

The `http` module is used to create an HTTP server.

```javascript
const http = require('http');

const server = http.createServer((req, res) => {
  res.statusCode = 200;
  res.setHeader('Content-Type', 'text/plain');
  res.end('Hello, World!\n');
});

server.listen(3000, () => {
  console.log('Server running at http://localhost:3000/');
});
```

### File System (fs)

The `fs` module allows you to work with the file system.

```javascript
const fs = require('fs');

// Write a file
fs.writeFileSync('example.txt', 'Hello, World!');

// Read a file
const data = fs.readFileSync('example.txt', 'utf8');
console.log(data); // Hello, World!

// Delete a file
fs.unlinkSync('example.txt');
```

### Path

The `path` module provides utilities for working with file and directory paths.

```javascript
const path = require('path');

const filePath = '/home/user/docs/file.txt';

console.log(path.dirname(filePath));  // /home/user/docs
console.log(path.basename(filePath)); // file.txt
console.log(path.extname(filePath));  // .txt
```

### Events

The `events` module allows you to work with events.

```javascript
const EventEmitter = require('events');
const eventEmitter = new EventEmitter();

eventEmitter.on('start', () => {
  console.log('Started');
});

eventEmitter.emit('start'); // Started
```

### Exercises

#### Exercise 1: Read, Write, and Delete a File

Create a script to read, write, and delete a file.

```javascript
const fs = require('fs');

// Write to file
fs.writeFile('example.txt', 'Hello, World!', (err) => {
  if (err) throw err;
  console.log('File written');

  // Read file
  fs.readFile('example.txt', 'utf8', (err, data) => {
    if (err) throw err;
    console.log(data); // Hello, World!

    // Delete file
    fs.unlink('example.txt', (err) => {
      if (err) throw err;
      console.log('File deleted');
    });
  });
});
```

---

## Global Objects

### Process

The `process` object provides information about the current Node.js process.

```javascript
console.log(`Current directory: ${process.cwd()}`);
console.log(`Current file: ${__filename}`);
```

### __dirname and __filename

These variables provide the current directory and file path.

```javascript
console.log(`Current directory: ${__dirname}`);
console.log(`Current file: ${__filename}`);
```

### Command-line Arguments

Use `process.argv` to handle command-line arguments.

```javascript
// Run: node app.js arg1 arg2
process.argv.forEach((val, index) => {
  console.log(`${index}: ${val}`);
});
```

### Exercises

#### Exercise 1: Log Current Directory and File Name

Create a script that logs the current directory and file name.

```javascript
console.log(`Current directory: ${__dirname}`);
console.log(`Current file: ${__filename}`);
```

#### Exercise 2: Command-Line Arguments

Use `process.argv` to accept command-line arguments and log them.

```javascript
const args = process.argv.slice(2);
console.log('Command-line arguments:', args);
```

---

## Express.js

### Request and Response Objects

Express.js provides `req` and `res` objects to handle HTTP requests and responses.

```javascript
const express = require('express');
const app = express();

app.get('/', (req, res) => {
  res.send('Hello, Express!');
});

app.listen(3000, () => {
  console.log('Server is running on port 3000');
});
```

### Routing

Define routes for different HTTP methods and use route parameters and query strings.

```javascript
app.get('/users/:id', (req, res) => {
  res.send(`User ID: ${req.params.id}`);
});

app.get('/search', (req, res) => {
  res.send(`Query: ${req.query.q}`);
});
```

### Middleware

Middleware functions are functions that have access to the request and response objects.

```javascript
// Custom middleware to log request details
app.use((req, res, next) => {
  console.log(`${req.method} ${req.url}`);
  next();
});

// Built-in middleware for parsing JSON
app.use(express.json());

// Error-handling middleware
app.use((err, req, res, next) => {
  res.status(500).send('Something went wrong!');
});
```

### Serving Static Files

Use the `express.static` middleware to serve static files.

```javascript
app.use(express.static('public'));
```

Create a directory named `public` and place your static files (HTML, CSS, JS) inside.

### Exercises

#### Exercise 1: Basic Express Server

Create an Express server that listens on a specific port and responds with "Hello, Express!" to requests.

```javascript
const

 express = require('express');
const app = express();

app.get('/', (req, res) => {
  res.send('Hello, Express!');
});

app.listen(3000, () => {
  console.log('Server is running on port 3000');
});
```

#### Exercise 2: CRUD API Routes

Implement routes for a basic CRUD API for a resource like users.

```javascript
const express = require('express');
const app = express();
app.use(express.json());

let users = [];

app.post('/users', (req, res) => {
  const user = req.body;
  users.push(user);
  res.status(201).json(user);
});

app.get('/users', (req, res) => {
  res.json(users);
});

app.put('/users/:id', (req, res) => {
  const id = req.params.id;
  const user = req.body;
  users[id] = user;
  res.json(user);
});

app.delete('/users/:id', (req, res) => {
  const id = req.params.id;
  users.splice(id, 1);
  res.status(204).end();
});

app.listen(3000, () => {
  console.log('Server is running on port 3000');
});
```

---

## Mongoose

### Schema Design and Validation

Define schemas with Mongoose and add validation rules.

```javascript
const mongoose = require('mongoose');
const Schema = mongoose.Schema;

const userSchema = new Schema({
  username: {
    type: String,
    required: true,
    unique: true
  },
  email: {
    type: String,
    required: true,
    unique: true
  },
  age: {
    type: Number,
    min: 0
  }
});

const User = mongoose.model('User', userSchema);
```

### User Authentication

Implement user authentication using JWT.

```javascript
const bcrypt = require('bcrypt');
const jwt = require('jsonwebtoken');

app.post('/register', async (req, res) => {
  const { username, password } = req.body;
  const hashedPassword = await bcrypt.hash(password, 10);
  const user = new User({ username, password: hashedPassword });
  await user.save();
  res.status(201).send('User registered');
});

app.post('/login', async (req, res) => {
  const { username, password } = req.body;
  const user = await User.findOne({ username });
  if (!user || !(await bcrypt.compare(password, user.password))) {
    return res.status(401).send('Invalid credentials');
  }
  const token = jwt.sign({ id: user._id }, 'your_jwt_secret', { expiresIn: '1h' });
  res.json({ token });
});
```

### Role-Based Access Control (RBAC)

Create middleware to check user roles.

```javascript
function checkRole(role) {
  return (req, res, next) => {
    if (req.user.role !== role) {
      return res.status(403).send('Access denied');
    }
    next();
  };
}

app.get('/admin', checkRole('admin'), (req, res) => {
  res.send('Admin area');
});
```

### Exercises

#### Exercise 1: Schema with Validation

Create a schema with validation for a resource like `User`.

```javascript
const mongoose = require('mongoose');
const Schema = mongoose.Schema;

const userSchema = new Schema({
  username: {
    type: String,
    required: [true, 'Username is required'],
    unique: true
  },
  email: {
    type: String,
    required: [true, 'Email is required'],
    unique: true
  },
  password: {
    type: String,
    required: [true, 'Password is required']
  },
  age: {
    type: Number,
    min: [0, 'Age must be positive']
  }
});

const User = mongoose.model('User', userSchema);
```

#### Exercise 2: Update Authentication to Hash Passwords

Update your authentication to hash passwords before storing them.

```javascript
userSchema.pre('save', async function(next) {
  if (this.isModified('password')) {
    this.password = await bcrypt.hash(this.password, 10);
  }
  next();
});
```

---

## Event Loop and Asynchronous Operations

### Exercises

#### Exercise 1: Measure Async Task Duration

Create a script to measure and log how long various asynchronous tasks take.

```javascript
const { hrtime } = require('process');

function measureAsyncTask(task) {
  const start = hrtime();
  task(() => {
    const end = hrtime(start);
    console.log(`Task took ${end[0]} seconds and ${end[1]} nanoseconds`);
  });
}

measureAsyncTask((callback) => {
  setTimeout(callback, 1000);
});
```

---

## Streams

### Exercises

#### Exercise 1: Read and Write Large Files Using Streams

Create a script that reads a large file in chunks and writes it to another file.

```javascript
const fs = require('fs');

const readStream = fs.createReadStream('largeFile.txt');
const writeStream = fs.createWriteStream('copyOfLargeFile.txt');

readStream.pipe(writeStream);

readStream.on('end', () => {
  console.log('File copied successfully');
});
```

---

## Clustering and Worker Threads

### Exercises

#### Exercise 1: Setup HTTP Server Cluster

Set up a simple cluster of HTTP servers to handle requests across multiple processes.

```javascript
const cluster = require('cluster');
const http = require('http');
const os = require('os');

if (cluster.isMaster) {
  const numCPUs = os.cpus().length;
  console.log(`Master ${process.pid} is running`);

  for (let i = 0; i < numCPUs; i++) {
    cluster.fork();
  }

  cluster.on('exit', (worker) => {
    console.log(`Worker ${worker.process.pid} died`);
  });
} else {
  http.createServer((req, res) => {
    res.writeHead(200);
    res.end('Hello, world!\n');
  }).listen(8000);

  console.log(`Worker ${process.pid} started`);
}
```

#### Exercise 2: Implement Worker Threads

Implement a worker thread that performs a CPU-intensive task.

```javascript
const { Worker } = require('worker_threads');

function runWorker(file) {
  return new Promise((resolve, reject) => {
    const worker = new Worker(file);
    worker.on('message', resolve);
    worker.on('error', reject);
    worker.on('exit', (code) => {
      if (code !== 0)
        reject(new Error(`Worker stopped with exit code ${code}`));
    });
  });
}

runWorker('./worker.js').then(console.log).catch(console.error);

// worker.js
const { parentPort } = require('worker_threads');
parentPort.postMessage('Hello from worker');
```

---

## Unit Testing

### Exercises

#### Exercise 1: Unit Tests with Mocha and Chai

Write unit tests for your functions using Mocha and Chai.

```javascript
// Using Mocha and Chai
const { expect } = require('chai');

function add(a, b) {
  return a + b;
}

describe('add', () => {
  it('should return the sum of two numbers', () => {
    expect(add(1, 2)).to.equal(3);
  });
});
```

---

## Integration and End-to-End Testing

### Exercises

#### Exercise 1: Integration Testing for API Endpoints

Write integration tests for your API endpoints using Supertest.

```javascript
const request = require('supertest');
const app = require('./app'); // Your Express app

describe('GET /', () => {
  it('should return Hello, Express!', (done) => {
    request(app)
      .get('/')
      .expect(200)
      .expect('Hello, Express!', done);
  });
});
```

#### Exercise 2: End-to-End Testing with Cypress

Set up Cypress to perform basic end-to-end tests on your application.

1. **Install Cypress**:

   ```bash
   npm install cypress --save-dev
   ```

2. **Create a Cypress Test**:

   ```javascript
   // cypress/integration/sample_spec.js
   describe('My First Test', () => {
     it('Visits the app root url', () => {
       cy.visit('/');
       cy.contains('Hello, Express!');
     });
   });
   ```

3. **Run Cypress**:

   ```bash
   npx cypress open
   ```

---

## Common Security Threats

### Exercises

#### Exercise 1: Secure Express Application

Secure your Express application using Helmet and rate limiting.

```javascript
const helmet = require('helmet');
const rateLimit = require('express-rate-limit');

app.use(helmet());

const limiter = rateLimit({
  windowMs: 15 * 60 * 1000, // 15 minutes
  max: 100, // Limit each IP to 100 requests per windowMs
});

app.use(limiter);
```

---

## Secure Authentication

### Exercises

#### Exercise 1: Update Authentication to Hash Passwords

Update your user authentication to hash passwords before storing them in the database.

```javascript
const bcrypt = require('bcrypt');
const jwt = require('jsonwebtoken');

app.post('/register', async (req, res) => {
  const { username, password } = req.body;
  const hashedPassword = await bcrypt.hash(password, 10);
  const user = new User({ username, password: hashedPassword });
  await user.save();
  res.status(201).send('User registered');
});
```

#### Exercise 2: Secure JWT Tokens

Secure JWT tokens by

 setting proper expiration and using HTTPS.

```javascript
const token = jwt.sign({ id: user._id }, 'your_jwt_secret', { expiresIn: '1h' });
```

---

## Profiling and Monitoring

### Exercises

#### Exercise 1: Profile Node.js Application

Profile your Node.js application to identify and resolve performance bottlenecks.

```javascript
const { exec } = require('child_process');

exec('node --inspect app.js', (err, stdout, stderr) => {
  if (err) {
    console.error(err);
    return;
  }
  console.log(`stdout: ${stdout}`);
  console.error(`stderr: ${stderr}`);
});
```



