# Scalable Web App with Authentication & Dashboard

A full-stack web application with JWT authentication, user profile management, and CRUD operations on tasks. Built with React.js frontend and Node.js/Express backend.

## Features

### Frontend
- Built with **React.js** (functional components with hooks)
- **Responsive design** using custom CSS
- **Client-side validation** on all forms
- **Protected routes** (login required for dashboard)
- Clean and simple UI

### Backend
- Built with **Node.js** and **Express**
- **JWT-based authentication** with token expiry
- **Password hashing** using bcryptjs
- **MongoDB** database with Mongoose ODM
- **Input validation** using express-validator
- RESTful API design

### Security
- Password hashing with bcrypt (10 rounds)
- JWT authentication middleware
- Protected API routes
- Input validation on both client and server
- Error handling and validation

### Dashboard Features
- User profile display and editing
- CRUD operations on tasks
- Search functionality
- Filter by status and priority
- Clean logout flow

## Project Structure

```
scalable-web-app/
├── backend/
│   ├── models/
│   │   ├── User.js          # User model with password hashing
│   │   └── Task.js          # Task model
│   ├── routes/
│   │   ├── auth.js          # Authentication routes
│   │   └── tasks.js         # Task CRUD routes
│   ├── middleware/
│   │   └── auth.js          # JWT authentication middleware
│   ├── server.js            # Express server
│   ├── package.json
│   └── .env
├── frontend/
│   ├── public/
│   │   └── index.html
│   ├── src/
│   │   ├── components/
│   │   │   ├── Navbar.js
│   │   │   └── PrivateRoute.js
│   │   ├── context/
│   │   │   └── AuthContext.js
│   │   ├── pages/
│   │   │   ├── Login.js
│   │   │   ├── Signup.js
│   │   │   └── Dashboard.js
│   │   ├── services/
│   │   │   └── api.js       # Axios configuration
│   │   ├── App.js
│   │   ├── index.js
│   │   └── index.css
│   └── package.json
└── README.md
```

## Tech Stack

### Frontend
- React.js 18
- React Router DOM 6
- Axios
- Custom CSS (responsive)

### Backend
- Node.js
- Express.js
- MongoDB with Mongoose
- JWT (jsonwebtoken)
- bcryptjs
- express-validator
- CORS

## Installation & Setup

### Prerequisites
- Node.js (v14 or higher)
- MongoDB (local or Atlas)
- npm or yarn

### 1. Clone the Repository
```bash
git clone <your-repo-url>
cd scalable-web-app
```

### 2. Backend Setup
```bash
cd backend
npm install

# Create .env file with:
PORT=5000
MONGODB_URI=mongodb://localhost:27017/scalable-web-app
JWT_SECRET=your_jwt_secret_key_change_in_production
NODE_ENV=development

# Start MongoDB (if running locally)
mongod

# Run the backend
npm start
# or for development with auto-reload
npm run dev
```

### 3. Frontend Setup
```bash
cd frontend
npm install

# Start the frontend
npm start
```

The app will open at `http://localhost:3000`

## API Endpoints

### Authentication Routes (`/api/auth`)

| Method | Endpoint | Description | Auth Required |
|--------|----------|-------------|---------------|
| POST | `/api/auth/signup` | Register new user | No |
| POST | `/api/auth/login` | Login user | No |
| GET | `/api/auth/profile` | Get user profile | Yes |
| PUT | `/api/auth/profile` | Update user profile | Yes |

### Task Routes (`/api/tasks`)

| Method | Endpoint | Description | Auth Required |
|--------|----------|-------------|---------------|
| GET | `/api/tasks` | Get all user tasks | Yes |
| GET | `/api/tasks/:id` | Get single task | Yes |
| POST | `/api/tasks` | Create new task | Yes |
| PUT | `/api/tasks/:id` | Update task | Yes |
| DELETE | `/api/tasks/:id` | Delete task | Yes |

### Query Parameters for GET `/api/tasks`
- `search` - Search in title and description
- `status` - Filter by status (pending, in-progress, completed)
- `priority` - Filter by priority (low, medium, high)

 API Request Examples

### 1. User Signup
```json
POST /api/auth/signup
{
  "name": "John Doe",
  "email": "john@example.com",
  "password": "password123"
}

Response:
{
  "success": true,
  "message": "User registered successfully",
  "token": "jwt_token_here",
  "user": {
    "id": "user_id",
    "name": "John Doe",
    "email": "john@example.com"
  }
}
```

### 2. User Login
```json
POST /api/auth/login
{
  "email": "john@example.com",
  "password": "password123"
}

Response:
{
  "success": true,
  "message": "Login successful",
  "token": "jwt_token_here",
  "user": {
    "id": "user_id",
    "name": "John Doe",
    "email": "john@example.com"
  }
}
```

### 3. Create Task
```json
POST /api/tasks
Headers: { "Authorization": "Bearer jwt_token_here" }
{
  "title": "Complete project",
  "description": "Finish the web app project",
  "status": "in-progress",
  "priority": "high"
}

Response:
{
  "success": true,
  "message": "Task created successfully",
  "task": {
    "_id": "task_id",
    "title": "Complete project",
    "description": "Finish the web app project",
    "status": "in-progress",
    "priority": "high",
    "userId": "user_id",
    "createdAt": "2024-01-01T00:00:00.000Z"
  }
}
```

 Security Features

1. **Password Hashing**
   - Uses bcryptjs with 10 salt rounds
   - Passwords are hashed before storing in database
   - Original passwords are never stored

2. **JWT Authentication**
   - Tokens expire after 7 days
   - Token validation on every protected route
   - Automatic logout on token expiry

3. **Input Validation**
   - Client-side validation using React
   - Server-side validation using express-validator
   - Prevents XSS and injection attacks

4. **Error Handling**
   - Proper error messages without exposing sensitive info
   - Try-catch blocks on all async operations
   - Graceful error responses

 Scalability Considerations

### Current Architecture
- **Modular code structure** - Easy to add new features
- **Separated concerns** - Models, routes, middleware, controllers
- **Reusable components** - Frontend components are modular
- **RESTful API** - Easy to integrate with mobile apps or other frontends

### Production Scaling Strategy

#### 1. Database Optimization
- Add **database indexing** on frequently queried fields (email, userId)
- Implement **connection pooling** for better performance
- Use **MongoDB Atlas** for managed, scalable database
- Add **caching layer** (Redis) for frequently accessed data

#### 2. Backend Scaling
- Deploy on **cloud platforms** (AWS, Heroku, DigitalOcean)
- Implement **load balancing** for multiple server instances
- Use **PM2** for process management and clustering
- Add **rate limiting** to prevent abuse
- Implement **API versioning** (/api/v1/)
- Add **logging** (Winston, Morgan) for monitoring
- Use **environment-specific configs**

#### 3. Frontend Scaling
- Implement **code splitting** and lazy loading
- Use **CDN** for static assets
- Add **service workers** for offline capability
- Implement **progressive web app** (PWA) features
- Use **state management** (Redux/Zustand) for complex state
- Add **error boundaries** for better error handling

#### 4. Security Enhancements
- Add **refresh tokens** for better token management
- Implement **OAuth** for social login
- Add **HTTPS** in production
- Implement **CSRF protection**
- Add **helmet.js** for security headers
- Use **rate limiting** on auth routes

#### 5. Performance
- Implement **pagination** for large datasets
- Add **data compression** (gzip)
- Use **image optimization**
- Implement **lazy loading** of images
- Add **performance monitoring** (New Relic, Datadog)

#### 6. DevOps
- Set up **CI/CD pipeline** (GitHub Actions, Jenkins)
- Implement **containerization** (Docker)
- Use **orchestration** (Kubernetes) for scaling
- Add **monitoring and alerts** (Prometheus, Grafana)
- Implement **automated testing** (Jest, Cypress)

 Testing

To add comprehensive testing:

### Backend Testing
```bash
npm install --save-dev jest supertest
# Add test scripts for API endpoints
```

### Frontend Testing
```bash
npm install --save-dev @testing-library/react @testing-library/jest-dom
# Add component and integration tests
```

 Deployment

### Backend Deployment (Heroku Example)
```bash
# Install Heroku CLI
heroku login
heroku create your-app-name-backend
git push heroku main
heroku config:set MONGODB_URI=your_mongo_uri
heroku config:set JWT_SECRET=your_secret
```

### Frontend Deployment (Vercel/Netlify)
```bash
# Build the app
npm run build

# Deploy to Vercel
vercel --prod

# Or Netlify
netlify deploy --prod
```

 Features for Mobile Scaling
- Responsive design already implemented
- Can be easily converted to React Native
- API is ready for mobile consumption
- JWT authentication works for mobile apps

 Contributing

1. Fork the repository
2. Create your feature branch (`git checkout -b feature/AmazingFeature`)
3. Commit your changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

 License

This project is open source and available under the MIT License.

# Author

**Your Name**
- Email: your.email@example.com
- GitHub: [@yourusername](https://github.com/yourusername)

## Acknowledgments

- Built as part of the Frontend Developer Intern assignment
- Thanks to PrimeTrade for the opportunity

---

**Note**: Remember to change the JWT_SECRET in production and never commit .env files to version control!
