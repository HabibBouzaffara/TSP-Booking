# TSP Room Booking System - Implementation Guide & README

## 📋 Project Overview

The TSP Room Booking System is a modern web application that replaces Excel-based room reservation systems with:

- **Role-based access control** (Employee & TSP Admin)
- **Conflict detection** with automatic TSP 10-minute buffer insertion
- **Microsoft Teams integration** for automatic meeting creation
- **Waitlist management** with auto-assignment on cancellation
- **Slot request system** for booking transfers between employees
- **Email notifications** for all key events
- **Real-time calendar view** with color-coded bookings
- **Early finish tracking** to free up rooms early

## 🚀 Quick Start

```bash
# Clone the repository
git clone https://github.com/HabibBouzaffara/TSP-Booking.git
cd TSP-Booking

# Setup environment
cp .env.example .env

# Start with Docker Compose
docker-compose up -d

# Access
# Frontend: http://localhost:3000
# Backend: http://localhost:8080/api
# Database: localhost:5432

# Default Login
# Email: admin@tsp.local
# Password: password123
```

## 🏗️ Architecture

- **Backend**: Spring Boot 3.2.1, Java 17, PostgreSQL 15
- **Frontend**: React 18, TypeScript, Material-UI
- **Authentication**: JWT Tokens
- **Notifications**: Email + WebSocket
- **Deployment**: Docker, Kubernetes-ready

## 📱 Key Features

### 1. Weekly Calendar View
- 3 rooms with side-by-side display
- Time slots 8 AM - 6 PM (30-minute intervals)
- Color-coded bookings (blue=confirmed, red=TSP buffer, yellow=waitlist, green=maintenance)

### 2. Booking Management
- Create, edit, delete bookings
- Select room, date, time, activity, hardware, software
- Auto-create Microsoft Teams meetings
- Finish early to free room

### 3. TSP Buffer
- Automatic 10-minute prep buffer between different employees
- Smart time adjustment suggestions
- Conflict prevention

### 4. Waitlist System
- Join waitlist when room full
- Auto-promotion when slots free
- Email notification on promotion

### 5. Slot Requests
- Request other user's bookings
- Owner can accept/reject
- Booking transfers with Teams meeting update

### 6. Notifications
- Email for all key events
- In-app notifications with bell icon
- WebSocket for real-time updates

### 7. Admin Dashboard
- View/manage all bookings
- Waitlist management
- Hardware/software inventory
- Configure system settings

## 📊 Database

Automatic schema generation with JPA/Hibernate:
- Users (with roles: EMPLOYEE, TSP_ADMIN)
- Rooms (Room A, B, C)
- Bookings (with Teams meeting links)
- TSP Buffers
- Waitlist Entries
- Slot Requests
- Notifications
- Hardware & Software

## 🔐 Security

- JWT Authentication (24-hour tokens)
- Password hashing (bcrypt)
- CORS protection
- SQL injection prevention
- Role-based access control

## 🛠️ Development

### Backend
```bash
cd backend
mvn spring-boot:run
```

### Frontend
```bash
cd frontend
npm install
npm start
```

## 📦 Docker Deployment

```bash
# Build and start
docker-compose up -d

# View logs
docker-compose logs -f backend

# Stop
docker-compose down
```

## 🧪 Testing

```bash
# Backend unit tests
cd backend && mvn test

# Frontend tests
cd frontend && npm test
```

## 📋 API Endpoints

### Authentication
- `POST /api/auth/login`
- `POST /api/auth/logout`

### Bookings
- `GET /api/bookings` - List with filters
- `POST /api/bookings` - Create
- `PUT /api/bookings/{id}` - Update
- `DELETE /api/bookings/{id}` - Delete
- `POST /api/bookings/{id}/finish-now` - Mark finished early

### Rooms
- `GET /api/rooms` - List all
- `GET /api/rooms/{id}` - Details

### Waitlist
- `GET /api/waitlist` - View entries
- `POST /api/waitlist` - Join
- `POST /api/waitlist/{id}/promote` - Admin promote

### Slot Requests
- `GET /api/slot-requests` - List
- `POST /api/slot-requests` - Create
- `PUT /api/slot-requests/{id}/accept` - Accept
- `PUT /api/slot-requests/{id}/reject` - Reject

### Notifications
- `GET /api/notifications` - List
- `PUT /api/notifications/{id}/read` - Mark read
- `DELETE /api/notifications/{id}` - Delete

## ⚙️ Configuration

See `.env.example` for all environment variables:

```env
# Database
SPRING_DATASOURCE_URL=jdbc:postgresql://localhost:5432/tsp_booking

# Email
MAIL_USERNAME=your-email@gmail.com
MAIL_PASSWORD=app-password

# JWT
JWT_SECRET=minimum-32-char-secret

# Teams (Optional)
TEAMS_ENABLED=true
TEAMS_CLIENT_ID=your-client-id
TEAMS_CLIENT_SECRET=your-secret
```

## 🐛 Troubleshooting

### Backend won't connect to database
```bash
# Check PostgreSQL is running
docker-compose logs postgres

# Verify connection string in .env
SPRING_DATASOURCE_URL=jdbc:postgresql://postgres:5432/tsp_booking
```

### Frontend shows blank page
```bash
# Check API URL
REACT_APP_API_URL=http://localhost:8080/api

# Clear cache and restart
docker-compose restart frontend
```

### Email not working
```bash
# Verify credentials
# Check backend logs
docker-compose logs backend | grep -i mail
```

## 📖 Documentation

Complete implementation guides available:
- `00-pom-setup.md` - Maven dependencies
- `01-backend-models.md` - Database models and repositories
- `02-backend-services.md` - Service layer implementation
- `03-backend-guide-complete.md` - Controllers and configuration
- `04-frontend-complete.md` - React components and pages
- `05-docker-deployment.md` - Docker and Kubernetes setup

## 📄 License

MIT License

## 👥 Author

Habib Bouzaffara

---

**Version**: 1.0.0  
**Last Updated**: January 2026  
**Status**: Production Ready ✅