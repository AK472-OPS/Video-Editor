# 🚀 Video Editor — Node.js, FFmpeg, Cpeak & Cluster

![Language](https://img.shields.io/badge/Language-JavaScript-yellow)
![Runtime](https://img.shields.io/badge/Runtime-Node.js-green)
![Video Processing](https://img.shields.io/badge/Video-FFmpeg-blue)
![Metadata](https://img.shields.io/badge/Metadata-FFprobe-orange)
![Architecture](https://img.shields.io/badge/Architecture-Cluster-purple)
![Status](https://img.shields.io/badge/Project-Completed-success)

A backend-driven **Video Editor application built with Node.js**, using a custom HTTP framework called **Cpeak**, **FFmpeg/FFprobe** for video processing, a **file-based database**, and **Node.js Cluster** for process-level parallelism.

The application provides functionality for:

* User authentication
* User profile management
* Video upload
* Video listing
* Automatic thumbnail generation
* Video dimension detection
* Audio extraction
* Video resizing
* Video asset serving
* Background resize job processing
* Resize progress tracking
* Multi-process execution using Node.js Cluster

---

# 📚 Table of Contents

* [Project Overview](#-project-overview)
* [Key Highlights](#-key-highlights)
* [Technologies Used](#️-technologies-used)
* [Features](#-features)
* [Project Structure](#-project-structure)
* [System Architecture](#-system-architecture)
* [Application Architecture](#-application-architecture)
* [Request Lifecycle](#-request-lifecycle)
* [Authentication Flow](#-authentication-flow)
* [Video Upload Flow](#-video-upload-flow)
* [Video Processing Pipeline](#-video-processing-pipeline)
* [Video Resize Flow](#-video-resize-flow)
* [Job Queue Architecture](#-job-queue-architecture)
* [Cluster Architecture](#-cluster-architecture)
* [FFmpeg & FFprobe Integration](#-ffmpeg--ffprobe-integration)
* [Database Architecture](#-database-architecture)
* [Storage Architecture](#-storage-architecture)
* [API Endpoints](#-api-endpoints)
* [Project Files](#-project-files)
* [Installation](#-installation)
* [Running the Application](#-running-the-application)
* [Cluster Mode](#-cluster-mode)
* [Sample Workflow](#-sample-workflow)
* [Error Handling](#-error-handling)
* [Supported Video Formats](#-supported-video-formats)
* [Current Limitations](#️-current-limitations)
* [Future Improvements](#-future-improvements)
* [Learning Outcomes](#-learning-outcomes)
* [Author](#-author)

---

# 📖 Project Overview

This project is a web-based video processing application developed using **Node.js**.

The backend handles user authentication, video uploads, video metadata, video assets, audio extraction, and video resizing.

Instead of using Express, the application uses a custom HTTP framework called **Cpeak**, located inside the `towrong` directory.

The application also integrates **FFmpeg** and **FFprobe** as external video-processing tools.

FFmpeg is used for operations such as:

* Generating thumbnails
* Extracting audio
* Resizing videos

FFprobe is used for obtaining:

* Video width
* Video height
* Video duration

The application stores application data in local files under the `data/` directory and stores uploaded and processed media under the `storage/` directory.

---

# ⭐ Key Highlights

* Node.js backend
* Custom Cpeak HTTP framework
* Middleware-based request processing
* Authentication using session tokens
* Cookie-based authentication
* File-based database
* Video upload using streams
* FFmpeg integration
* FFprobe integration
* Automatic thumbnail generation
* Video dimension detection
* Audio extraction
* Video resizing
* Resize progress tracking
* Job queue for resize operations
* Node.js Cluster support
* Automatic worker replacement after worker failure
* Static frontend serving
* REST-style API routes

---

# 🛠️ Technologies Used

| Technology              | Purpose                      |
| ----------------------- | ---------------------------- |
| **Node.js**             | Backend runtime              |
| **JavaScript**          | Application development      |
| **Cpeak**               | Custom HTTP server/framework |
| **FFmpeg**              | Video processing             |
| **FFprobe**             | Video metadata extraction    |
| **Node.js Cluster**     | Multi-process execution      |
| **Node.js Streams**     | Video file upload/download   |
| **File System API**     | File storage and database    |
| **Nodemon**             | Development server restart   |
| **HTML/CSS/JavaScript** | Frontend                     |

---

# ✨ Features

| Feature                   | Status |
| ------------------------- | ------ |
| User Login                | ✅      |
| User Logout               | ✅      |
| User Information          | ✅      |
| Update User               | ✅      |
| Video Upload              | ✅      |
| Video Listing             | ✅      |
| Thumbnail Generation      | ✅      |
| Video Dimension Detection | ✅      |
| Audio Extraction          | ✅      |
| Video Resize              | ✅      |
| Resize Progress Tracking  | ✅      |
| Video Asset Serving       | ✅      |
| Session Management        | ✅      |
| File-Based Database       | ✅      |
| FFmpeg Integration        | ✅      |
| FFprobe Integration       | ✅      |
| Job Queue                 | ✅      |
| Node.js Cluster           | ✅      |
| Static File Serving       | ✅      |

---

# 📂 Project Structure

```text
Video Editor/
│
├── data/
│   ├── sessions
│   ├── users
│   └── videos
│
├── public/
│   ├── index.html
│   ├── scripts.js
│   └── styles.css
│
├── src/
│   ├── controllers/
│   │   ├── user.js
│   │   └── video.js
│   │
│   ├── lib/
│   │   ├── FF.js
│   │   ├── JobQueue.js
│   │   └── util.js
│   │
│   ├── middleware/
│   │   └── index.js
│   │
│   ├── cluster.js
│   ├── DB.js
│   ├── index.js
│   └── router.js
│
├── storage/
│   └── <videoId>/
│       ├── original.<extension>
│       ├── thumbnail.jpg
│       ├── audio.aac
│       └── <width>x<height>.<extension>
│
├── towrong/
│   ├── lib/
│   │   ├── index.ts
│   │   └── types.ts
│   ├── dist/
│   ├── test/
│   ├── package.json
│   └── tsconfig.json
│
├── package.json
├── package-lock.json
└── .gitignore
```

---

# 🏗️ System Architecture

```text
                         ┌─────────────────────┐
                         │       Browser       │
                         │   Frontend Client   │
                         └──────────┬──────────┘
                                    │
                                    │ HTTP Request
                                    ▼
                         ┌─────────────────────┐
                         │       Cpeak         │
                         │   HTTP Framework    │
                         └──────────┬──────────┘
                                    │
                                    ▼
                         ┌─────────────────────┐
                         │     Middleware      │
                         │                     │
                         │ • Static Files      │
                         │ • JSON Parsing      │
                         │ • Authentication    │
                         │ • Server Index      │
                         └──────────┬──────────┘
                                    │
                                    ▼
                         ┌─────────────────────┐
                         │       Router        │
                         └──────────┬──────────┘
                                    │
                     ┌──────────────┴──────────────┐
                     │                             │
                     ▼                             ▼
             ┌───────────────┐             ┌───────────────┐
             │ User Controller│             │Video Controller│
             └───────┬───────┘             └───────┬───────┘
                     │                             │
                     ▼                             ▼
              ┌────────────┐               ┌───────────────┐
              │    DB      │               │    FFmpeg     │
              │ File Store │               │   FFprobe     │
              └────────────┘               └───────┬───────┘
                                                    │
                                                    ▼
                                             ┌─────────────┐
                                             │   Storage   │
                                             │ Video Files │
                                             └─────────────┘
```

---

# 🧩 Application Architecture

The application is divided into several logical layers.

```text
                  Client
                    │
                    ▼
             Cpeak HTTP Server
                    │
                    ▼
               Middleware
                    │
                    ▼
                 Router
                    │
          ┌─────────┴─────────┐
          │                   │
          ▼                   ▼
    User Controller      Video Controller
          │                   │
          ▼                   ▼
       Database         FFmpeg / FFprobe
          │                   │
          ▼                   ▼
       data/              storage/
```

### Main Components

### 1. Cpeak

Cpeak is the custom HTTP framework used by the project instead of Express.

It provides functionality such as:

* HTTP server creation
* Routing
* Middleware
* JSON parsing
* Static file serving
* Response helpers
* Error handling
* URL parameter extraction

---

### 2. Middleware

The application registers middleware for:

* Static file serving
* JSON request parsing
* Authentication
* Serving `index.html` for selected frontend routes

---

### 3. Router

The router maps HTTP methods and paths to controllers.

Example:

```text
POST   /api/login
DELETE /api/logout
GET    /api/user
PUT    /api/user

GET    /api/videos
POST   /api/upload-video
PUT    /api/video/resize
PATCH  /api/video/extract-audio

GET    /get-video-asset
```

---

### 4. Controllers

Two main controller modules are present:

```text
controllers/
│
├── user.js
└── video.js
```

`user.js` handles:

* Login
* Logout
* User information
* User updates

`video.js` handles:

* Video listing
* Upload
* Resize
* Audio extraction
* Asset serving

---

# 🔄 Request Lifecycle

```text
Browser
   │
   │ HTTP Request
   ▼
Cpeak Server
   │
   ▼
Static File Middleware
   │
   ▼
JSON Parser
   │
   ▼
Authentication Middleware
   │
   ▼
Server Index Middleware
   │
   ▼
Router
   │
   ▼
Controller
   │
   ├──────────────► DB
   │
   ├──────────────► FFmpeg
   │
   ├──────────────► FFprobe
   │
   └──────────────► Storage
   │
   ▼
HTTP Response
   │
   ▼
Browser
```

---

# 🔐 Authentication Flow

The application uses a session-token-based authentication mechanism.

```text
             User
              │
              ▼
       POST /api/login
              │
              ▼
     Check Username/Password
              │
        ┌─────┴─────┐
        │           │
      Invalid      Valid
        │           │
        ▼           ▼
      401        Generate Token
                    │
                    ▼
             Store Session
                    │
                    ▼
             Set Cookie
                    │
                    ▼
              Login Success
```

The token is stored in the session data.

For protected routes:

```text
Request
   │
   ▼
Read Cookie
   │
   ▼
Find Session Token
   │
   ├───────────────┐
   │               │
 Found           Not Found
   │               │
   ▼               ▼
Set req.userId    401 Unauthorized
   │
   ▼
Continue Request
```

---

# 👤 User Operations

The project supports:

### Login

```text
POST /api/login
```

The server:

1. Reads username and password.
2. Searches the users data.
3. Validates the password.
4. Generates a token.
5. Stores the session.
6. Sends the token through a cookie.

---

### Logout

```text
DELETE /api/logout
```

The server:

1. Finds the current user's session.
2. Removes the session.
3. Saves the updated database.
4. Deletes the authentication cookie.

---

### Get User Information

```text
GET /api/user
```

Returns the authenticated user's:

* Username
* Name

---

### Update User

```text
PUT /api/user
```

Allows updating:

* Username
* Name
* Password

The password is updated only when a password value is provided.

---

# 🎬 Video Upload Flow

```text
Browser
   │
   │ POST /api/upload-video
   │
   ▼
Authentication
   │
   ▼
Read Filename Header
   │
   ▼
Validate Extension
   │
   ▼
Generate Video ID
   │
   ▼
Create Storage Directory
   │
   ▼
Stream Uploaded Video
   │
   ▼
Save Original Video
   │
   ▼
FFmpeg
   │
   ├──────────────► Generate Thumbnail
   │
   ▼
FFprobe
   │
   └──────────────► Get Dimensions
   │
   ▼
Update Video Database
   │
   ▼
Return Success Response
```

---

# 📹 Video Upload Processing

When a video is uploaded, the server performs the following operations:

### Step 1 — Read Filename

The filename is read from the request header:

```text
filename
```

### Step 2 — Validate Format

Supported extensions:

```text
mp4
mkv
avi
mov
flv
wmv
```

### Step 3 — Generate Video ID

A random video ID is generated using Node.js crypto utilities.

### Step 4 — Create Storage Directory

```text
storage/<videoId>/
```

### Step 5 — Save Original Video

```text
original.<extension>
```

### Step 6 — Generate Thumbnail

FFmpeg generates:

```text
thumbnail.jpg
```

### Step 7 — Detect Dimensions

FFprobe obtains:

```text
width
height
```

### Step 8 — Store Metadata

The video information is stored in:

```text
data/videos
```

---

# 🎞️ Video Processing Pipeline

```text
                  Uploaded Video
                         │
                         ▼
                ┌────────────────┐
                │ Original Video │
                └───────┬────────┘
                        │
            ┌───────────┼───────────┐
            │           │           │
            ▼           ▼           ▼
        FFmpeg       FFprobe     FFmpeg
            │           │           │
            ▼           ▼           ▼
       Thumbnail    Dimensions   Audio
            │                       │
            │                       ▼
            │                  audio.aac
            │
            ▼
      thumbnail.jpg
```

---

# 🎵 Audio Extraction

The project supports extracting audio from an uploaded video.

Endpoint:

```text
PATCH /api/video/extract-audio
```

Processing:

```text
Original Video
      │
      ▼
    FFmpeg
      │
      │ -vn
      │ -c:a copy
      ▼
  audio.aac
```

The project prevents audio extraction when the stored video metadata indicates that audio has already been extracted.

---

# 📐 Video Resize Flow

Video resizing is handled through the job queue and cluster architecture.

```text
Browser
   │
   │ PUT /api/video/resize
   ▼
Video Controller
   │
   ▼
Create Resize Entry
   │
   ▼
Send Resize Job
   │
   ▼
Cluster Primary
   │
   ▼
Job Queue
   │
   ▼
Execute Resize Job
   │
   ▼
FFmpeg
   │
   ▼
Generate Resized Video
   │
   ▼
Update Progress
   │
   ▼
Update Database
```

---

# 📦 Job Queue Architecture

The project contains a custom `JobQueue` implementation.

```text
Resize Request
      │
      ▼
Create Job
      │
      ▼
JobQueue.enqueue()
      │
      ▼
Jobs Array
      │
      ▼
JobQueue.dequeue()
      │
      ▼
executeNext()
      │
      ▼
execute()
      │
      ▼
FFmpeg Resize
      │
      ▼
Update Progress
      │
      ▼
Mark Job Complete
      │
      ▼
Execute Next Job
```

The queue maintains:

```text
jobs
currentJob
```

The queue processes one current job at a time within its instance.

---

# 📊 Resize Progress Tracking

During FFmpeg execution, the application reads FFmpeg's progress information from `stderr`.

```text
FFmpeg Output
      │
      ▼
Read time=
      │
      ▼
Calculate Current Time
      │
      ▼
Calculate Percentage
      │
      ▼
Update Database
      │
      ▼
Store:
"0%"
"25%"
"50%"
"75%"
...
"100%"
```

The resize status is stored under the corresponding resolution in the video's `resizes` object.

Example:

```json
{
  "resizes": {
    "640x360": {
      "processing": "50%"
    }
  }
}
```

---

# ⚙️ Cluster Architecture

The project supports Node.js Cluster mode through:

```text
src/cluster.js
```

Architecture:

```text
                    ┌──────────────────┐
                    │  Primary Process │
                    └────────┬─────────┘
                             │
              ┌──────────────┼──────────────┐
              │              │              │
              ▼              ▼              ▼
          Worker 1       Worker 2       Worker 3
              │              │              │
              └──────────────┼──────────────┘
                             │
                        HTTP Server
```

The primary process:

1. Creates the `JobQueue`.
2. Determines available parallelism.
3. Forks worker processes.
4. Receives resize messages from workers.
5. Adds resize jobs to the queue.
6. Replaces workers that exit.

---

# 🔒 Process Isolation

Node.js Cluster creates separate worker processes.

Conceptually:

```text
Primary Process
      │
      ├── Worker Process
      │
      ├── Worker Process
      │
      ├── Worker Process
      │
      └── Worker Process
```

Each worker runs the application server through:

```text
src/index.js
```

The primary process is responsible for coordination while workers handle HTTP requests.

---

# 🔁 Resize Job Communication

The resize request is passed from the worker toward the primary process.

```text
Worker
  │
  │ messageType = "new-resize"
  │
  ▼
Primary Process
  │
  ▼
JobQueue.enqueue()
  │
  ▼
Resize Job
```

The job contains:

```text
videoId
width
height
```

---

# 🧯 Worker Failure Handling

The cluster implementation listens for worker exit events.

```text
Worker Process
      │
      ▼
Worker Exits
      │
      ▼
Cluster detects exit
      │
      ▼
cluster.fork()
      │
      ▼
New Worker Created
```

This allows the cluster to replace an exited worker.

---

# 🎥 FFmpeg & FFprobe Integration

The project uses:

```text
ffmpeg-static
ffprobe-static
```

FFmpeg is invoked using Node.js:

```text
child_process.spawn()
```

---

## FFmpeg Operations

The project contains functions for:

### Thumbnail Generation

```text
makeThumbnail()
```

### Audio Extraction

```text
extractAudio()
```

### Video Resizing

```text
resizeVideo()
```

---

## FFprobe Operations

The project uses FFprobe for:

### Video Dimensions

```text
getDimenstions()
```

### Video Duration

```text
getDuration()
```

---

# 🖼 Thumbnail Generation

The thumbnail is generated using FFmpeg from the uploaded video.

```text
Video
 │
 ▼
FFmpeg
 │
 │ Select frame
 ▼
thumbnail.jpg
```

The thumbnail is stored inside the video's storage directory.

---

# 🗃️ Database Architecture

The project uses a simple **file-based database** implemented in:

```text
src/DB.js
```

Three data files are maintained:

```text
data/
│
├── users
├── sessions
└── videos
```

The database is loaded using Node.js file-system operations.

---

# 👥 Users Data

The `data/users` file stores user information such as:

```text
id
name
username
password
```

---

# 🔑 Sessions Data

The `data/sessions` file stores:

```text
userId
token
```

This is used by the authentication middleware to identify logged-in users.

---

# 🎬 Videos Data

The `data/videos` file stores metadata such as:

```text
id
videoId
name
extension
dimensions
userId
extractedAudio
resizes
```

---

# 💾 Database Operations

The database class provides:

```text
update()
save()
```

### update()

Reads the latest contents from:

```text
data/users
data/sessions
data/videos
```

### save()

Writes the updated data back to the same files.

---

# 📁 Storage Architecture

Uploaded and processed media are stored under:

```text
storage/
```

Each video gets its own directory.

Example:

```text
storage/
└── 1ef94f33/
    ├── original.mp4
    ├── thumbnail.jpg
    ├── audio.aac
    └── 640x360.mp4
```

---

# 📡 Video Asset Serving

The application provides:

```text
GET /get-video-asset
```

The controller can serve different types of assets including:

```text
thumbnail
audio
resize
original
```

The server sets appropriate response headers such as:

```text
Content-Type
Content-Length
Content-Disposition
```

and streams the file to the client.

---

# 🌐 API Endpoints

## Authentication & User APIs

| Method | Endpoint      | Purpose                      |
| ------ | ------------- | ---------------------------- |
| POST   | `/api/login`  | Login user                   |
| DELETE | `/api/logout` | Logout user                  |
| GET    | `/api/user`   | Get current user information |
| PUT    | `/api/user`   | Update user information      |

---

## Video APIs

| Method | Endpoint                   | Purpose                          |
| ------ | -------------------------- | -------------------------------- |
| GET    | `/api/videos`              | Get videos belonging to the user |
| POST   | `/api/upload-video`        | Upload a video                   |
| PUT    | `/api/video/resize`        | Request video resize             |
| PATCH  | `/api/video/extract-audio` | Extract audio                    |
| GET    | `/get-video-asset`         | Retrieve video-related assets    |

---

# 🧭 Frontend Routes

The middleware serves `index.html` for:

```text
GET /
GET /login
GET /profile
```

Static frontend files are served from:

```text
public/
```

---

# 🧱 Cpeak Framework

The project contains a custom HTTP framework inside:

```text
towrong/
```

The framework source is written in TypeScript.

Important files include:

```text
towrong/
│
├── lib/
│   ├── index.ts
│   └── types.ts
│
├── test/
├── dist/
├── package.json
└── tsconfig.json
```

Cpeak provides functionality such as:

* HTTP server creation
* Route registration
* Middleware
* Route middleware
* JSON parsing
* Static file serving
* File sending
* Redirects
* JSON responses
* Status codes
* URL variable extraction
* Query parameter parsing
* Error handling

---

# 🔀 Cpeak Routing Flow

```text
Incoming Request
       │
       ▼
Cpeak HTTP Server
       │
       ▼
Global Middleware
       │
       ▼
Find HTTP Method
       │
       ▼
Match Route Regex
       │
       ▼
Extract URL Variables
       │
       ▼
Route Middleware
       │
       ▼
Route Handler
       │
       ▼
Response
```

---

# 🧩 Middleware Architecture

The application registers middleware in `src/index.js`.

```text
Request
  │
  ▼
serveStatic()
  │
  ▼
parseJSON
  │
  ▼
authenticate()
  │
  ▼
serverIndex()
  │
  ▼
Router
```

---

# ⚠️ Error Handling

The application registers a centralized error handler.

```text
Error
  │
  ▼
Cpeak Error Handler
  │
  ├───────────────┐
  │               │
Known Error    Unknown Error
  │               │
  ▼               ▼
Use Status     HTTP 500
Code
  │
  ▼
JSON Response
```

Known errors containing a status are returned with that status code.

Unexpected errors result in an HTTP 500 response.

---

# 🔄 Complete Video Processing Workflow

```text
                         USER
                          │
                          ▼
                    Login / Session
                          │
                          ▼
                   Upload Video
                          │
                          ▼
                 Validate File Type
                          │
                          ▼
                  Generate Video ID
                          │
                          ▼
                  Save Original File
                          │
                ┌─────────┴─────────┐
                │                   │
                ▼                   ▼
             FFmpeg              FFprobe
                │                   │
                ▼                   ▼
           Thumbnail            Dimensions
                │                   │
                └─────────┬─────────┘
                          ▼
                    Save Metadata
                          │
              ┌───────────┴────────────┐
              │                        │
              ▼                        ▼
        Extract Audio             Resize Video
              │                        │
              ▼                        ▼
          audio.aac              Job Queue
                                       │
                                       ▼
                                  FFmpeg Resize
                                       │
                                       ▼
                                  Progress Update
                                       │
                                       ▼
                                  Save Result
```

---

# ▶️ Installation

## 1. Clone the Repository

```bash
git clone <your-repository-url>
```

## 2. Enter the Project Directory

```bash
cd "Video Editor"
```

## 3. Install Dependencies

```bash
npm install
```

The project uses:

```text
ffmpeg-static
ffprobe-static
nodemon
```

---

# ▶️ Running the Application

Start the normal development server:

```bash
npm start
```

The application starts on:

```text
http://localhost:8060
```

The port is defined in:

```text
src/index.js
```

as:

```text
8060
```

---

# ⚡ Running in Cluster Mode

The project provides a separate cluster script:

```bash
npm run cluster
```

This starts:

```text
src/cluster.js
```

The cluster determines the available parallelism and creates worker processes accordingly.

---

# 🧪 Development

The project uses **Nodemon** for development.

The package scripts are:

```text
npm start
npm run cluster
npm test
```

The current `npm test` script in `package.json` is a placeholder that exits with:

```text
Error: no test specified
```

---

# 🖥️ Sample Application Workflow

```text
1. Open the application
          │
          ▼
2. Login
          │
          ▼
3. Authentication Cookie Created
          │
          ▼
4. Upload Video
          │
          ▼
5. Thumbnail Generated
          │
          ▼
6. Video Dimensions Detected
          │
          ▼
7. Video Appears in User's Video List
          │
          ▼
8. Extract Audio OR Resize Video
          │
          ▼
9. Processed Asset Stored
          │
          ▼
10. Asset Can Be Retrieved
```

---

# 📦 Supported Video Formats

The upload controller currently accepts:

```text
MP4
MKV
AVI
MOV
FLV
WMV
```

The extension is extracted from the uploaded filename and validated before processing.

---

# ⚠️ Current Limitations

The current implementation has several limitations:

* Uses a file-based database instead of a production database.
* Authentication uses simple session tokens stored in files.
* Passwords are stored directly in the users data file.
* The JobQueue is an in-memory queue.
* Resize jobs are handled by the queue instance in the cluster primary process.
* The project is designed primarily as a learning/project implementation rather than a production deployment.
* The current `npm test` command is not configured to execute the framework tests.
* The resize endpoint communicates with the cluster through `process.send()`.

---

# 🚀 Future Improvements

Possible improvements for a production-ready version include:

* PostgreSQL or MongoDB instead of file-based storage
* Redis-backed job queue
* Secure password hashing
* JWT or stronger session management
* Object storage for videos
* Distributed video-processing workers
* Docker deployment
* Reverse proxy
* Rate limiting
* Input validation
* Better authentication security
* Persistent job state
* Job retry mechanism
* Job cancellation
* Real-time processing updates
* Production logging
* Monitoring and metrics
* Horizontal scaling
* Better error recovery
* Automated testing for the main application

---

# 🎯 Learning Outcomes

This project provides practical experience with:

* Node.js
* HTTP servers
* Custom web framework development
* Middleware architecture
* Routing
* REST-style APIs
* Authentication
* Cookies and sessions
* File-system operations
* Node.js streams
* FFmpeg
* FFprobe
* Child processes
* Video processing
* Job queues
* Process isolation
* Node.js Cluster
* Multi-process architecture
* Error handling
* File-based data persistence

---

# 🧠 Important Technical Concepts

This project demonstrates several important backend and system-level concepts.

### Node.js

Used as the backend runtime for handling HTTP requests, file operations, processes, and asynchronous operations.

### Cpeak

A custom HTTP framework used to implement:

* Routing
* Middleware
* Request/response handling
* Static files
* JSON parsing
* Error handling

### FFmpeg

Used as the actual video-processing engine.

### FFprobe

Used to inspect video metadata.

### Child Process

Node.js `spawn()` is used to execute FFmpeg and FFprobe as separate processes.

### Job Queue

Used to organize resize jobs before execution.

### Cluster

Used to create multiple Node.js worker processes.

### File System

Used for:

* Database persistence
* Video storage
* Audio storage
* Thumbnail storage
* Resized video storage

---

# 🏛️ High-Level Architecture Summary

```text
                         ┌──────────────────────┐
                         │      Frontend        │
                         │ HTML/CSS/JavaScript  │
                         └──────────┬───────────┘
                                    │
                                    ▼
                         ┌──────────────────────┐
                         │       Cpeak          │
                         │   HTTP Framework     │
                         └──────────┬───────────┘
                                    │
                                    ▼
                         ┌──────────────────────┐
                         │     Middleware       │
                         └──────────┬───────────┘
                                    │
                                    ▼
                         ┌──────────────────────┐
                         │       Router         │
                         └──────────┬───────────┘
                                    │
                    ┌───────────────┴───────────────┐
                    │                               │
                    ▼                               ▼
            ┌──────────────┐                ┌──────────────┐
            │ User         │                │ Video        │
            │ Controller   │                │ Controller   │
            └──────┬───────┘                └──────┬───────┘
                   │                               │
                   ▼                               ▼
              ┌─────────┐                 ┌────────────────┐
              │   DB    │                 │ FFmpeg/FFprobe │
              └────┬────┘                 └───────┬────────┘
                   │                              │
                   ▼                              ▼
              data/                          storage/
```

---

# 🔍 Project Design Summary

```text
                Presentation Layer
                       │
                       ▼
                public/index.html
                public/scripts.js
                public/styles.css
                       │
                       ▼
                  HTTP Layer
                       │
                       ▼
                    Cpeak
                       │
                       ▼
                 Middleware
                       │
                       ▼
                  Router Layer
                       │
             ┌─────────┴─────────┐
             ▼                   ▼
       User Controller     Video Controller
             │                   │
             ▼                   ▼
            DB             FFmpeg / FFprobe
             │                   │
             ▼                   ▼
         data files           storage files
```

---

# 👨‍💻 Author

**Akhil Kumar Singh**

**B.Tech (Computer Science Engineering)**

### Skills Demonstrated

* JavaScript
* Node.js
* Backend Development
* HTTP Server Development
* Custom Framework Development
* Cpeak
* FFmpeg
* FFprobe
* Node.js Cluster
* Job Queue
* File System
* REST APIs
* Authentication
* Video Processing
* System Design

---

# 🤝 Support

If you found this project useful or learned something from it, please consider giving it a ⭐ on GitHub. Your support helps improve the project and motivates future enhancements.

---

# ⭐ Project Highlights

```text
Node.js
   +
Custom Cpeak HTTP Framework
   +
FFmpeg / FFprobe
   +
File-Based Database
   +
Job Queue
   +
Node.js Cluster
   +
Video Processing
   =
Complete Video Processing Backend
```
