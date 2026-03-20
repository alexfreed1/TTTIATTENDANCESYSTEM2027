# Backend API Documentation

FastAPI-based REST API for Unit Attendance System.

## 🚀 Quick Start

```bash
# Install dependencies
pip install -r requirements.txt

# Create .env file
cp .env.example .env
# Edit .env with your Supabase credentials

# Run server
python main.py
```

Server runs on: `http://localhost:8000`
Interactive docs: `http://localhost:8000/docs`

## 📁 Project Structure

```
BACKEND/
├── main.py              # FastAPI app and route imports
├── config.py            # Settings and environment config
├── requirements.txt     # Python dependencies
├── .env.example         # Environment variables template
├── Procfile             # Render deployment
├── render.yaml          # Render config
└── routes/
    ├── auth.py          # Authentication (signup, login)
    ├── attendance.py    # Attendance marking and retrieval
    ├── classes_routes.py # Class management
    ├── students.py      # Student management
    └── departments.py   # Department management
```

## 🔑 Environment Variables

Create `.env` file with:

```env
SUPABASE_URL=https://your-project.supabase.co
SUPABASE_ANON_KEY=your-anon-key
SUPABASE_SERVICE_ROLE_KEY=your-service-role-key
DEBUG=True
PORT=8000
HOST=0.0.0.0
FRONTEND_URL=http://localhost:3000
```

## 📊 API Routes

### Health Check
```
GET /        # Welcome message
GET /health  # System health check
```

### Authentication (`/api/auth`)

#### Login
```http
POST /api/auth/login
Content-Type: application/json

{
    "email": "trainer@example.com",
    "password": "password123"
}
```

**Response:**
```json
{
    "access_token": "eyJ...",
    "user": {
        "id": "uuid",
        "email": "trainer@example.com",
        "name": "Trainer Name",
        "department_id": 1
    },
    "message": "Login successful"
}
```

#### Signup
```http
POST /api/auth/signup
Content-Type: application/json

{
    "email": "trainer@example.com",
    "password": "password123",
    "full_name": "Trainer Name",
    "department_id": 1
}
```

**Response:**
```json
{
    "message": "Signup successful. Please verify your email.",
    "user_id": "uuid"
}
```

#### Logout
```http
POST /api/auth/logout
Authorization: Bearer {token}
```

#### Get Current User
```http
GET /api/auth/me
Authorization: Bearer {token}
```

---

### Attendance (`/api/attendance`)

#### Submit Attendance
```http
POST /api/attendance/submit
Authorization: Bearer {token}
Content-Type: application/json

{
    "trainer_id": "uuid",
    "class_id": 1,
    "unit_id": 1,
    "week": 1,
    "lesson": "L1",
    "records": [
        {
            "student_id": 1,
            "unit_id": 1,
            "week": 1,
            "lesson": "L1",
            "status": "present"
        }
    ]
}
```

**Response:**
```json
{
    "message": "Attendance submitted successfully",
    "records_saved": 25
}
```

#### Get Student Attendance
```http
GET /api/attendance/student/1?unit_id=1&week=1
Authorization: Bearer {token}
```

**Response:**
```json
{
    "student_id": 1,
    "records": [
        {
            "id": 1,
            "student_id": 1,
            "status": "present",
            "week": 1,
            "lesson": "L1"
        }
    ]
}
```

#### Get Class Attendance
```http
GET /api/attendance/class/1?unit_id=1&week=1&lesson=L1
Authorization: Bearer {token}
```

#### Update Attendance
```http
PUT /api/attendance/1
Authorization: Bearer {token}
Content-Type: application/json

{
    "status": "absent"
}
```

#### Delete Attendance
```http
DELETE /api/attendance/1
Authorization: Bearer {token}
```

#### Get Attendance Summary
```http
GET /api/attendance/summary/{trainer_id}?unit_id=1&class_id=1
Authorization: Bearer {token}
```

---

### Classes (`/api/classes`)

#### Create Class
```http
POST /api/classes
Authorization: Bearer {token}
Content-Type: application/json

{
    "name": "Class A",
    "department_id": 1
}
```

#### Get All Classes
```http
GET /api/classes?department_id=1
Authorization: Bearer {token}
```

#### Get Single Class
```http
GET /api/classes/1
Authorization: Bearer {token}
```

#### Update Class
```http
PUT /api/classes/1
Authorization: Bearer {token}
Content-Type: application/json

{
    "name": "Class A Updated",
    "department_id": 1
}
```

#### Delete Class
```http
DELETE /api/classes/1
Authorization: Bearer {token}
```

#### Get Class Units
```http
GET /api/classes/1/units
Authorization: Bearer {token}
```

#### Assign Unit to Class
```http
POST /api/classes/units/assign
Authorization: Bearer {token}
Content-Type: application/json

{
    "class_id": 1,
    "unit_id": 1,
    "trainer_id": "uuid"
}
```

---

### Students (`/api/students`)

#### Create Student
```http
POST /api/students
Authorization: Bearer {token}
Content-Type: application/json

{
    "full_name": "John Doe",
    "admission_number": "ADM001",
    "email": "student@example.com",
    "class_id": 1
}
```

#### Get All Students
```http
GET /api/students?class_id=1
Authorization: Bearer {token}
```

#### Get Students in Class
```http
GET /api/students/class/1
Authorization: Bearer {token}
```

**Response:**
```json
{
    "class_id": 1,
    "students": [
        {
            "id": 1,
            "full_name": "John Doe",
            "admission_number": "ADM001",
            "class_id": 1
        }
    ],
    "count": 25
}
```

#### Get Single Student
```http
GET /api/students/1
Authorization: Bearer {token}
```

#### Update Student
```http
PUT /api/students/1
Authorization: Bearer {token}
Content-Type: application/json

{
    "full_name": "John Doe Updated",
    "email": "new@example.com"
}
```

#### Delete Student
```http
DELETE /api/students/1
Authorization: Bearer {token}
```

---

### Departments (`/api/departments`)

#### Create Department
```http
POST /api/departments
Authorization: Bearer {token}
Content-Type: application/json

{
    "name": "Information Technology"
}
```

#### Get All Departments
```http
GET /api/departments
Authorization: Bearer {token}
```

**Response:**
```json
{
    "departments": [
        {
            "id": 1,
            "name": "Information Technology"
        }
    ]
}
```

#### Get Single Department
```http
GET /api/departments/1
Authorization: Bearer {token}
```

#### Update Department
```http
PUT /api/departments/1
Authorization: Bearer {token}
Content-Type: application/json

{
    "name": "IT Department"
}
```

#### Delete Department
```http
DELETE /api/departments/1
Authorization: Bearer {token}
```

#### Get Department Classes
```http
GET /api/departments/1/classes
Authorization: Bearer {token}
```

---

## 🔐 Status Codes

| Code | Meaning |
|------|---------|
| 200 | OK - Successful request |
| 201 | Created - Resource created |
| 400 | Bad Request - Invalid data |
| 401 | Unauthorized - Missing/invalid auth |
| 404 | Not Found - Resource doesn't exist |
| 500 | Internal Server Error |

---

## 🧪 Testing with cURL

```bash
# Login
curl -X POST http://localhost:8000/api/auth/login \
  -H "Content-Type: application/json" \
  -d '{"email":"trainer@example.com","password":"password123"}'

# Get departments
curl -X GET http://localhost:8000/api/departments \
  -H "Authorization: Bearer YOUR_TOKEN"

# Get students in class
curl -X GET http://localhost:8000/api/students/class/1 \
  -H "Authorization: Bearer YOUR_TOKEN"
```

---

## 🚀 Deployment to Render

1. Push code to GitHub
2. Go to render.com
3. Create new Web Service
4. Connect GitHub repository
5. Set:
   - Build: `pip install -r requirements.txt`
   - Start: `uvicorn main:app --host 0.0.0.0 --port $PORT`
6. Add environment variables from Supabase
7. Deploy

---

## 📝 Notes

- All timestamps are in ISO 8601 format
- Attendance status: `present` or `absent`
- Lessons: `L1`, `L2`, `L3`, `L4`
- Weeks: 1-52
- All IDs are integers except `trainer_id` which is UUID

---

## 🆘 Troubleshooting

**CORS Error?**
- Check `FRONTEND_URL` in .env
- Verify CORS middleware in main.py

**Database Connection Error?**
- Verify Supabase credentials
- Check network access
- Test connection in Supabase dashboard

**Authentication Failed?**
- Confirm email is registered
- Check password length (min 6 chars)
- Verify Supabase Auth enabled

---

**Built with ❤️ using FastAPI**
#   T T T I A T T E N D A N C E S Y S T E M 2 0 2 7  
 