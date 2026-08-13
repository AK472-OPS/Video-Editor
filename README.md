# 🎬 Video Editor — Node.js, Cpeak, FFmpeg & Cluster

A web-based **Video Processing and Editing application** built using **Node.js**, a custom HTTP framework called **Cpeak**, **FFmpeg**, **FFprobe**, **Node.js Cluster**, a custom **Job Queue**, and a **file-based database**.

The application provides user authentication, video uploading, video listing, thumbnail generation, video metadata extraction, audio extraction, video resizing, video asset serving, and multi-process execution using Node.js Cluster.

---

## ⭐ Project Highlights

- 🚀 Node.js backend
- 🌐 Custom Cpeak HTTP framework
- 🔀 Custom routing and middleware
- 🔐 User authentication and session management
- 🍪 Cookie-based authentication
- 📤 Video upload using Node.js streams
- 🎞️ FFmpeg video processing
- 🔍 FFprobe video metadata extraction
- 🖼️ Automatic thumbnail generation
- 📐 Video dimension detection
- 🎵 Audio extraction
- 📏 Video resizing
- 📊 Resize progress tracking
- 📦 Custom in-memory Job Queue
- ⚙️ Node.js Cluster support
- 👷 Multiple worker processes
- 💾 File-based database
- 📁 File-system based media storage
- 📡 Video asset serving
- 🖥️ HTML/CSS/JavaScript frontend

---

# 📖 Table of Contents

- [Project Overview](#-project-overview)
- [Features](#-features)
- [Technologies Used](#️-technologies-used)
- [Project Structure](#-project-structure)
- [System Architecture](#-system-architecture)
- [Application Architecture](#-application-architecture)
- [Request Lifecycle](#-request-lifecycle)
- [Authentication Flow](#-authentication-flow)
- [Video Upload Flow](#-video-upload-flow)
- [Video Processing Pipeline](#-video-processing-pipeline)
- [FFmpeg and FFprobe](#-ffmpeg-and-ffprobe)
- [Audio Extraction](#-audio-extraction)
- [Video Resize](#-video-resize)
- [Job Queue](#-job-queue)
- [Cluster Architecture](#️-cluster-architecture)
- [Process Isolation](#-process-isolation)
- [Database Architecture](#️-database-architecture)
- [Storage Architecture](#-storage-architecture)
- [API Endpoints](#-api-endpoints)
- [Cpeak Framework](#-cpeak-framework)
- [Middleware Architecture](#-middleware-architecture)
- [Error Handling](#️-error-handling)
- [Supported Video Formats](#-supported-video-formats)
- [Installation](#️-installation)
- [Running the Application](#-running-the-application)
- [Running in Cluster Mode](#-running-in-cluster-mode)
- [Application Workflow](#-application-workflow)
- [Current Limitations](#️-current-limitations)
- [Future Improvements](#-future-improvements)
- [Learning Outcomes](#-learning-outcomes)
- [Author](#-author)

---

# 📖 Project Overview

This project is a web-based video processing application developed using **Node.js**.

The backend handles:

- User authentication
- User sessions
- Video uploads
- Video metadata
- Video listing
- Thumbnail generation
- Audio extraction
- Video resizing
- Video asset serving

Instead of using Express, the project uses a custom HTTP framework called **Cpeak**, located inside the `towrong/` directory.

The project integrates **FFmpeg** and **FFprobe** as external media-processing tools.

### FFmpeg is used for:

- Thumbnail generation
- Audio extraction
- Video resizing

### FFprobe is used for:

- Video width detection
- Video height detection
- Video duration detection

Application data is stored inside the `data/` directory, while uploaded and processed media files are stored inside `storage/`.

---

# ✨ Features

| Feature | Status |
|---|---|
| User Login | ✅ |
| User Logout | ✅ |
| User Information | ✅ |
| Update User | ✅ |
| Session Management | ✅ |
| Video Upload | ✅ |
| Video Listing | ✅ |
| Thumbnail Generation | ✅ |
| Video Dimension Detection | ✅ |
| Video Duration Detection | ✅ |
| Audio Extraction | ✅ |
| Video Resize | ✅ |
| Resize Progress Tracking | ✅ |
| Video Asset Serving | ✅ |
| File-Based Database | ✅ |
| FFmpeg Integration | ✅ |
| FFprobe Integration | ✅ |
| Job Queue | ✅ |
| Node.js Cluster | ✅ |
| Static File Serving | ✅ |
| Middleware | ✅ |
| Custom Routing | ✅ |

---

# 🛠️ Technologies Used

| Technology | Purpose |
|---|---|
| **Node.js** | Backend runtime |
| **JavaScript** | Application development |
| **Cpeak** | Custom HTTP framework |
| **FFmpeg** | Video processing |
| **FFprobe** | Video metadata extraction |
| **Node.js Cluster** | Multi-process execution |
| **Node.js Streams** | Video file upload and streaming |
| **File System API** | Database and media storage |
| **Nodemon** | Development workflow |
| **HTML** | Frontend structure |
| **CSS** | Frontend styling |
| **JavaScript** | Frontend interaction |

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
│   │
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
🏗️ System Architecture
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
                    ┌───────────────┴───────────────┐
                    │                               │
                    ▼                               ▼
             ┌───────────────┐              ┌────────────────┐
             │ User Controller│              │Video Controller│
             └───────┬───────┘              └───────┬────────┘
                     │                              │
                     ▼                              ▼
              ┌────────────┐               ┌────────────────┐
              │ File-Based │               │ FFmpeg/FFprobe │
              │     DB     │               └───────┬────────┘
              └──────┬─────┘                       │
                     │                             ▼
                     ▼                       ┌────────────┐
                  data/                      │  storage/  │
                                             │   Media    │
                                             └────────────┘
🧩 Application Architecture

The application is divided into logical components:

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
             ┌────────┴────────┐
             │                 │
             ▼                 ▼
      User Controller    Video Controller
             │                 │
             ▼                 ▼
        File-Based DB     FFmpeg / FFprobe
             │                 │
             ▼                 ▼
           data/            storage/
Main Components
1. Cpeak

Custom HTTP framework responsible for HTTP server functionality, routing, middleware, JSON parsing, static files, response helpers, and error handling.

2. Middleware

Handles:

Static files
JSON parsing
Authentication
Frontend index serving
3. Router

Maps HTTP methods and URLs to their respective handlers.

4. Controllers

Two main controllers are used:

controllers/
├── user.js
└── video.js

user.js handles user-related operations.

video.js handles video-related operations.

🔄 Request Lifecycle
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
   ├──────────────► File-Based DB
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
🔐 Authentication Flow

The application uses session tokens and cookies for authentication.

                    User
                     │
                     ▼
              POST /api/login
                     │
                     ▼
             Validate Credentials
                     │
                ┌────┴────┐
                │         │
              Invalid    Valid
                │         │
                ▼         ▼
              401       Generate Token
                          │
                          ▼
                   Store Session
                          │
                          ▼
                    Set Cookie
                          │
                          ▼
                    Login Success

For authenticated requests:

Request
   │
   ▼
Read Authentication Cookie
   │
   ▼
Find Session
   │
   ├───────────────┐
   │               │
 Found          Not Found
   │               │
   ▼               ▼
Continue       Unauthorized
🎬 Video Upload Flow
Browser
   │
   │ POST /api/upload-video
   ▼
Authentication / Request Handling
   │
   ▼
Read Filename
   │
   ▼
Validate Extension
   │
   ▼
Generate Video ID
   │
   ▼
Create Video Storage Directory
   │
   ▼
Stream Uploaded Video
   │
   ▼
Save Original Video
   │
   ├──────────────► FFmpeg
   │                    │
   │                    ▼
   │              Generate Thumbnail
   │
   └──────────────► FFprobe
                        │
                        ▼
                 Extract Metadata
                        │
                        ▼
                  Save Video Data
🎞️ Video Processing Pipeline
                    Uploaded Video
                           │
                           ▼
                    Original Video
                           │
              ┌────────────┼────────────┐
              │            │            │
              ▼            ▼            ▼
           FFmpeg       FFprobe       FFmpeg
              │            │            │
              ▼            ▼            ▼
         Thumbnail     Dimensions     Audio
              │                         │
              ▼                         ▼
       thumbnail.jpg                audio.aac
🎥 FFmpeg and FFprobe

The project uses:

ffmpeg-static
ffprobe-static

FFmpeg and FFprobe are executed from Node.js using:

child_process.spawn()
FFmpeg Operations
Thumbnail Generation
makeThumbnail()

Flow:

Video
  │
  ▼
FFmpeg
  │
  ▼
thumbnail.jpg
Audio Extraction
extractAudio()

Flow:

Original Video
      │
      ▼
    FFmpeg
      │
      ▼
  audio.aac
Video Resizing
resizeVideo()

FFmpeg is used to generate a resized video according to the requested width and height.

FFprobe Operations
Video Dimensions
getDimenstions()

Used to obtain video width and height.

Video Duration
getDuration()

Used to obtain video duration.

🎵 Audio Extraction

Endpoint:

PATCH /api/video/extract-audio

Processing flow:

Original Video
      │
      ▼
    FFmpeg
      │
      ▼
  audio.aac
      │
      ▼
Storage
📐 Video Resize

Endpoint:

PUT /api/video/resize

The resize request contains:

videoId
width
height

The resize architecture is designed around the Job Queue and Cluster.

Client
  │
  ▼
Resize Request
  │
  ▼
Video Controller
  │
  ▼
Resize Message
  │
  ▼
Cluster Primary
  │
  ▼
JobQueue
  │
  ▼
Resize Job
  │
  ▼
FFmpeg
  │
  ▼
Resized Video
📊 Resize Progress Tracking

During FFmpeg processing, progress information is read from FFmpeg output.

The processing status is maintained for the requested resolution.

Example:

{
  "resizes": {
    "640x360": {
      "processing": "50%"
    }
  }
}

The progress value represents the current processing state for the selected resize operation.

📦 Job Queue

The project contains a custom in-memory JobQueue implementation.

Location:

src/lib/JobQueue.js

The queue maintains:

Pending Jobs
Current Job

Resize jobs contain:

videoId
width
height
Queue Flow
Resize Request
      │
      ▼
Create Resize Job
      │
      ▼
JobQueue
      │
      ▼
Queue Job
      │
      ▼
Resize Processing
      │
      ▼
FFmpeg
      │
      ▼
Progress Update

The queue is implemented in memory and is intended for the current project architecture.

⚙️ Cluster Architecture

The project supports Node.js Cluster mode through:

src/cluster.js

Architecture:

                    ┌──────────────────┐
                    │  Primary Process │
                    │    cluster.js    │
                    └────────┬─────────┘
                             │
               ┌─────────────┼─────────────┐
               │             │             │
               ▼             ▼             ▼
           Worker 1      Worker 2      Worker 3
               │             │             │
               └─────────────┼─────────────┘
                             │
                             ▼
                       HTTP Server

The primary process:

Determines available parallelism.
Creates worker processes.
Coordinates resize messages.
Maintains the Job Queue.
Replaces workers when they exit.
🔁 Resize Job Communication

In cluster mode, the worker sends a resize message to the primary process.

Worker
   │
   │ process.send()
   │
   │ messageType = "new-resize"
   ▼
Primary Process
   │
   ▼
JobQueue.enqueue()
   │
   ▼
Resize Job

The resize message contains:

videoId
width
height
🔒 Process Isolation

Node.js Cluster creates separate worker processes.

Primary Process
      │
      ├── Worker Process
      │
      ├── Worker Process
      │
      ├── Worker Process
      │
      └── Worker Process

Each worker runs the application server.

The primary process is responsible for coordination while worker processes handle HTTP requests.

🧯 Worker Failure Handling

The cluster listens for worker exit events.

Worker Process
      │
      ▼
Worker Exits
      │
      ▼
Cluster Detects Exit
      │
      ▼
cluster.fork()
      │
      ▼
New Worker Created

This allows an exited worker to be replaced.

🗃️ Database Architecture

The project uses a simple file-based database implemented in:

src/DB.js

The database maintains three files:

data/
├── users
├── sessions
└── videos
👤 Users Data

The users data contains information such as:

id
name
username
password
🔑 Sessions Data

The sessions data contains:

userId
token

Sessions are used by the authentication system.

🎬 Videos Data

The videos data contains metadata such as:

id
videoId
name
extension
dimensions
userId
extractedAudio
resizes
💾 Database Operations

The database provides operations for updating and saving data.

update()
save()
update()

Loads the latest data from:

data/users
data/sessions
data/videos
save()

Writes updated data back to the corresponding files.

📁 Storage Architecture

Uploaded and processed media are stored under:

storage/

Each video has its own directory.

Example:

storage/
└── <videoId>/
    ├── original.mp4
    ├── thumbnail.jpg
    ├── audio.aac
    └── 640x360.mp4
📡 Video Asset Serving

Endpoint:

GET /get-video-asset

The application can serve:

Original Video
Thumbnail
Audio
Resized Video

The server uses appropriate response headers such as:

Content-Type
Content-Length
Content-Disposition

and streams the requested file to the client.

🌐 API Endpoints
Authentication & User APIs
Method	Endpoint	Purpose
POST	/api/login	Login user
DELETE	/api/logout	Logout user
GET	/api/user	Get current user information
PUT	/api/user	Update user information
Video APIs
Method	Endpoint	Purpose
GET	/api/videos	Get videos
POST	/api/upload-video	Upload video
PUT	/api/video/resize	Request video resize
PATCH	/api/video/extract-audio	Extract audio
GET	/get-video-asset	Retrieve video assets
🧭 Frontend Routes

The frontend is served from:

public/

The application provides frontend routes including:

GET /
GET /login
GET /profile

Static frontend files include:

index.html
scripts.js
styles.css
🧱 Cpeak Framework

The project contains a custom HTTP framework inside:

towrong/

The framework source is written in TypeScript.

Important files include:

towrong/
│
├── lib/
│   ├── index.ts
│   └── types.ts
│
├── dist/
├── test/
├── package.json
└── tsconfig.json

Cpeak provides functionality such as:

HTTP server creation
Route registration
Middleware
Route middleware
JSON parsing
Static file serving
File sending
Redirects
JSON responses
Status codes
URL variable extraction
Query parameter parsing
Error handling
🔀 Cpeak Routing Flow
Incoming Request
       │
       ▼
Cpeak HTTP Server
       │
       ▼
Global Middleware
       │
       ▼
Route Matching
       │
       ▼
Route Middleware
       │
       ▼
Route Handler
       │
       ▼
Controller
       │
       ▼
HTTP Response
🧩 Middleware Architecture

The application uses middleware for:

Request
   │
   ▼
Static File Serving
   │
   ▼
JSON Parsing
   │
   ▼
Authentication
   │
   ▼
Frontend Index Handling
   │
   ▼
Router
⚠️ Error Handling

The application contains centralized error handling.

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
Status Code     HTTP 500
  │               │
  └───────┬───────┘
          ▼
     JSON Response

Unexpected server errors are handled through the HTTP error handling mechanism.

📦 Supported Video Formats

The upload controller accepts the following extensions:

.mp4
.mkv
.avi
.mov
.flv
.wmv

The file extension is extracted from the uploaded filename and validated before processing.

🔧 Installation
1. Clone the Repository
git clone <YOUR-GITHUB-REPOSITORY-URL>
2. Open the Project
cd <PROJECT-DIRECTORY>
3. Install Dependencies
npm install
▶️ Running the Application

Start the normal server:

npm start

The application uses port:

8060

Open:

http://localhost:8060
⚡ Running in Cluster Mode

To run the project using Node.js Cluster:

npm run cluster

This starts:

src/cluster.js

The cluster creates worker processes according to the available parallelism.

🧪 Available Scripts

The project provides the following npm scripts:

npm start
npm run cluster
npm test

npm start runs the normal application server.

npm run cluster starts the cluster architecture.

The current test script is a placeholder and is not configured as the application's automated test suite.

🖥️ Complete Application Workflow
                         USER
                          │
                          ▼
                   Login / Session
                          │
                          ▼
                    Upload Video
                          │
                          ▼
                  Validate Format
                          │
                          ▼
                   Generate Video ID
                          │
                          ▼
                  Save Original Video
                          │
                ┌─────────┴─────────┐
                │                   │
                ▼                   ▼
             FFmpeg              FFprobe
                │                   │
                ▼                   ▼
           Thumbnail            Metadata
                │                   │
                └─────────┬─────────┘
                          ▼
                    Save Video Data
                          │
              ┌───────────┴────────────┐
              │                        │
              ▼                        ▼
        Extract Audio             Resize Video
              │                        │
              ▼                        ▼
          audio.aac                 JobQueue
                                       │
                                       ▼
                                    FFmpeg
                                       │
                                       ▼
                                Processing Status
                                       │
                                       ▼
                                  Storage
🏛️ High-Level Design
                ┌─────────────────────────┐
                │        Frontend         │
                │ HTML / CSS / JavaScript │
                └────────────┬────────────┘
                             │
                             ▼
                    ┌────────────────┐
                    │     Cpeak      │
                    │  HTTP Server   │
                    └───────┬────────┘
                            │
                            ▼
                       Middleware
                            │
                            ▼
                         Router
                            │
              ┌─────────────┴─────────────┐
              │                           │
              ▼                           ▼
       User Controller             Video Controller
              │                           │
              ▼                           ▼
         File-Based DB             FFmpeg / FFprobe
              │                           │
              ▼                           ▼
           data/                       storage/
⚠️ Current Limitations

The current implementation has some limitations:

Uses a file-based database instead of a production database.
Session information is stored in local files.
The Job Queue is an in-memory queue.
The project is intended as a project/learning implementation rather than a production deployment.
The current npm test script is not configured as a complete application test suite.
Resize requests use cluster process communication through process.send() in the cluster architecture.
Media storage is local rather than object storage.
🚀 Future Improvements

Possible future improvements include:

PostgreSQL or MongoDB
Redis-backed Job Queue
Secure password hashing
Stronger authentication/session management
Cloud/object storage
Distributed video-processing workers
Docker deployment
Reverse proxy
Rate limiting
Better input validation
Persistent job state
Job retry mechanism
Job cancellation
Real-time processing updates
Production logging
Monitoring and metrics
Horizontal scaling
Automated testing
Improved error recovery
🎯 Learning Outcomes

This project demonstrates practical experience with:

Node.js
HTTP servers
Custom HTTP framework development
Cpeak
Middleware architecture
Routing
REST-style APIs
Authentication
Cookies and sessions
Node.js Streams
File-system operations
FFmpeg
FFprobe
Child processes
Video processing
Job queues
Node.js Cluster
Process isolation
Multi-process architecture
Error handling
File-based data persistence
🧠 Core Technical Concepts
Node.js

Used as the backend runtime for HTTP requests, file operations, process management, and asynchronous operations.

Cpeak

Custom HTTP framework responsible for the application's HTTP server, routing, middleware, JSON handling, static files, and response handling.

FFmpeg

Used as the actual media-processing engine.

FFprobe

Used for extracting video metadata.

Child Processes

FFmpeg and FFprobe are executed as external processes using Node.js spawn().

Job Queue

Used to organize resize jobs in the current architecture.

Node.js Cluster

Used to create multiple worker processes and coordinate processing-related communication.

File System

Used for:

Database persistence
Original video storage
Thumbnail storage
Audio storage
Resized video storage
📌 Why This Project Is Interesting

The project combines multiple backend and system-level concepts in a single application:

Node.js
    +
Custom HTTP Framework
    +
Middleware
    +
Routing
    +
Authentication
    +
File-Based Database
    +
Streams
    +
FFmpeg / FFprobe
    +
Job Queue
    +
Node.js Cluster
    +
Video Processing
    │
    ▼
🎬 Video Editor

This makes the project useful for understanding how a backend application can integrate HTTP handling, file processing, external processes, background jobs, and multi-process execution.

👨‍💻 Author

Akhil Kumar Singh

B.Tech — Computer Science Engineering

Technologies & Concepts
JavaScript
Node.js
Cpeak
FFmpeg
FFprobe
Node.js Cluster
Job Queue
File System
REST APIs
Authentication
Video Processing
Backend Development
System Design
⭐ Support

If you found this project useful or interesting, consider giving the repository a ⭐ on GitHub!

Your support is appreciated and motivates further development.

⭐ Project Summary
             Node.js
                +
       Custom Cpeak Framework
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
                │
                ▼
        🎬 Video Editor


