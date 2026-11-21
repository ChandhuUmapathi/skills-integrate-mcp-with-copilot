# Mergington High School Activities API

A super simple FastAPI application that allows students to view extracurricular activities and teachers to manage registrations.

## Features

- View all available extracurricular activities
- Teacher login to manage student registrations
- Students can view activities and participants without modifying registrations

## Getting Started

1. Install the dependencies:

   ```
   pip install fastapi uvicorn
   ```

2. Configure teacher credentials. Do not commit credentials to the repository.

   PowerShell:

   ```powershell
   $env:TEACHER_USERNAME = "teacher"
   $env:TEACHER_PASSWORD = "choose-a-strong-password"
   ```

   Bash:

   ```bash
   export TEACHER_USERNAME=teacher
   export TEACHER_PASSWORD='choose-a-strong-password'
   ```

3. Start the application from the repository root:

   ```
   uvicorn app:app --app-dir src --reload
   ```

4. Open your browser and go to:
   - API documentation: http://localhost:8000/docs
   - Alternative documentation: http://localhost:8000/redoc

Teacher sessions expire after eight hours and are held in memory, so server restarts log out all teachers. Use HTTPS when deploying the application; the session cookie is marked secure on HTTPS requests.

## API Endpoints

| Method | Endpoint                                                          | Description                                                         |
| ------ | ----------------------------------------------------------------- | ------------------------------------------------------------------- |
| GET    | `/activities`                                                     | Get all activities with their details and current participant count |
| GET    | `/auth/status`                                                    | Check whether the current browser has an active teacher session     |
| POST   | `/auth/login`                                                     | Log in with the configured teacher username and password            |
| POST   | `/auth/logout`                                                    | End the current teacher session                                     |
| POST   | `/activities/{activity_name}/signup?email=student@mergington.edu` | Register a student (teacher login required)                         |
| DELETE | `/activities/{activity_name}/unregister?email=student@mergington.edu` | Unregister a student (teacher login required)                    |

## Data Model

The application uses a simple data model with meaningful identifiers:

1. **Activities** - Uses activity name as identifier:

   - Description
   - Schedule
   - Maximum number of participants allowed
   - List of student emails who are signed up

2. **Students** - Uses email as identifier:
   - Name
   - Grade level

All data is stored in memory, which means data will be reset when the server restarts.
