# TSP Room Booking System - Complete Implementation Guide

## Overview

This guide explains how to build the complete TSP Room Booking System from the generated code.

## Project Files Included

The following markdown files contain all implementation code:

1. **00-pom-setup.md** - Maven pom.xml with all dependencies
2. **01-backend-models.md** - All JPA entities and repositories
3. **02-backend-services.md** - Service layer with business logic
4. **03-backend-guide-complete.md** - Controllers, config, DTOs, exceptions
5. **04-frontend-complete.md** - React components and pages
6. **05-docker-deployment.md** - Docker, docker-compose, Kubernetes
7. **06-README.md** - Complete project documentation

## Step-by-Step Build Process

### 1. Setup Backend

```bash
# Create directory structure
mkdir -p backend/src/main/java/com/tsp/booking/{config,controller,dto,exception,model,repository,security,service,util}
mkdir -p backend/src/main/resources
mkdir -p backend/src/test/java/com/tsp/booking/{controller,service,integration}

# Copy pom.xml from 00-pom-setup.md
cp 00-pom-setup.md/pom.xml backend/pom.xml

# Copy Dockerfile
cp 05-docker-deployment.md/backend/Dockerfile backend/Dockerfile
```

### 2. Create Model Classes (from 01-backend-models.md)

```bash
# Copy all entity classes to backend/src/main/java/com/tsp/booking/model/
cp User.java Room.java Booking.java TspBuffer.java WaitlistEntry.java SlotRequest.java Notification.java Hardware.java Software.java

# Copy all repository interfaces to backend/src/main/java/com/tsp/booking/repository/
cp *Repository.java
```

### 3. Create Service Classes (from 02-backend-services.md & 03-backend-guide-complete.md)

```bash
# Copy all service classes to backend/src/main/java/com/tsp/booking/service/
cp BookingService.java ConflictDetectionService.java WaitlistService.java EmailService.java TeamsIntegrationService.java NotificationService.java SlotRequestService.java AuthService.java RoomService.java UserService.java
```

### 4. Create Controller Classes (from 03-backend-guide-complete.md)

```bash
# Copy all controller classes to backend/src/main/java/com/tsp/booking/controller/
cp *Controller.java

# Controllers needed:
# - AuthController.java
# - BookingController.java
# - RoomController.java
# - WaitlistController.java
# - SlotRequestController.java
# - NotificationController.java
# - SettingsController.java
```

### 5. Create Security & Configuration (from 03-backend-guide-complete.md)

```bash
# Copy config classes to backend/src/main/java/com/tsp/booking/config/
cp SecurityConfig.java CorsConfig.java WebSocketConfig.java

# Copy security classes to backend/src/main/java/com/tsp/booking/security/
cp JwtTokenProvider.java JwtAuthenticationFilter.java CustomUserDetailsService.java AuthEntryPoint.java
```

### 6. Create DTOs & Exceptions

```bash
# Copy DTOs to backend/src/main/java/com/tsp/booking/dto/
cp BookingRequest.java BookingResponse.java WaitlistRequest.java SlotRequestDTO.java AuthRequest.java AuthResponse.java NotificationResponse.java UserResponse.java

# Copy exceptions to backend/src/main/java/com/tsp/booking/exception/
cp ConflictException.java ResourceNotFoundException.java UnauthorizedException.java ValidationException.java GlobalExceptionHandler.java
```

### 7. Copy Utility Classes

```bash
# Copy to backend/src/main/java/com/tsp/booking/util/
cp DateTimeUtil.java ConflictDetector.java
```

### 8. Setup Application Properties

```bash
# Copy from 05-docker-deployment.md
cp application.yml backend/src/main/resources/
cp application-dev.yml backend/src/main/resources/
cp application-prod.yml backend/src/main/resources/
```

### 9. Copy Main Application Class

```bash
# Copy TspBookingApplication.java to backend/src/main/java/com/tsp/booking/
cp TspBookingApplication.java
```

### 10. Setup Frontend

```bash
# Create directory structure
mkdir -p frontend/src/{api,components,context,hooks,pages,styles,types}
mkdir -p frontend/public

# Copy package.json from 04-frontend-complete.md
cp package.json frontend/
cp tsconfig.json frontend/

# Copy index.html
echo '<div id="root"></div>' > frontend/public/index.html

# Copy Dockerfile
cp frontend/Dockerfile frontend/

# Copy nginx config
cp frontend/nginx.conf frontend/
```

### 11. Create Frontend Components (from 04-frontend-complete.md)

```bash
# Copy API layer
cp src/api/*.ts frontend/src/api/

# Copy components
cp src/components/*.tsx frontend/src/components/

# Copy pages
cp src/pages/*.tsx frontend/src/pages/

# Copy context
cp src/context/*.tsx frontend/src/context/

# Copy hooks
cp src/hooks/*.ts frontend/src/hooks/

# Copy types
cp src/types/index.ts frontend/src/types/

# Copy main files
cp src/App.tsx src/index.tsx frontend/src/
cp src/styles/globals.css frontend/src/styles/
```

### 12. Setup Docker

```bash
# Copy from 05-docker-deployment.md
cp docker-compose.yml .
cp init.sql .
```

### 13. Build & Test

```bash
# Backend build
cd backend
mvn clean install
mvn test

# Frontend setup
cd frontend
npm install
npm run build

# Start with Docker
docker-compose up -d
```

## Implementation Checklist

### Backend
- [ ] Maven dependencies installed
- [ ] All model classes created
- [ ] All repositories created
- [ ] All services implemented
- [ ] All controllers created
- [ ] Security configuration done
- [ ] DTOs and exceptions created
- [ ] Application properties configured
- [ ] Database schema initialized
- [ ] Unit tests passing
- [ ] Integration tests passing

### Frontend
- [ ] Dependencies installed
- [ ] API client setup
- [ ] Context and hooks created
- [ ] Components built
- [ ] Pages implemented
- [ ] Routing configured
- [ ] Styling applied
- [ ] Build successful
- [ ] Tests passing

### Deployment
- [ ] Backend Dockerfile created
- [ ] Frontend Dockerfile created
- [ ] docker-compose.yml configured
- [ ] Environment variables set
- [ ] Database initialization script ready
- [ ] Nginx configuration done
- [ ] Docker images build successfully
- [ ] Services start without errors
- [ ] Application accessible at http://localhost:3000

## Key Implementation Details

### Database Auto-Configuration
JPA/Hibernate automatically creates all tables based on model classes. The `init.sql` script adds initial data.

### JWT Authentication Flow
1. User logs in with email/password
2. Backend generates JWT token
3. Frontend stores token in localStorage
4. Token sent with every API request in Authorization header
5. Backend validates token on each request

### Conflict Detection Algorithm
```
When creating booking:
1. Check for overlapping confirmed bookings
2. If conflict found, throw ConflictException
3. Find previous booking in same room
4. If different employee, insert TSP buffer
5. Suggest adjusted time if needed
6. Save booking and buffer
```

### Waitlist Auto-Assignment
```
When cancelling booking:
1. Set booking status to CANCELLED
2. Find waitlist entries for this room
3. Check compatibility (date, time preferences)
4. For first compatible entry:
   - Create new booking
   - Set waitlist entry to ASSIGNED
   - Send confirmation email
   - Send notification
```

### Email Notification Triggers
- Booking created
- Booking updated
- Booking cancelled
- Waitlist joined
- Waitlist promoted
- Room freed early
- Slot request received
- Slot request accepted
- Slot request rejected

## Testing the System

### Test Login Credentials
Default users created by init.sql:

```
Administrator:
Email: admin@tsp.local
Password: password123

Employee 1:
Email: john@example.com
Password: password123

Employee 2:
Email: jane@example.com
Password: password123
```

### Test Scenarios

1. **Create Booking**
   - Login as employee
   - Select room, date, time
   - Fill in activity, hardware, software
   - Verify booking appears on calendar

2. **Test Conflict Detection**
   - Create first booking
   - Try to book overlapping time
   - Verify conflict message

3. **Test TSP Buffer**
   - Create booking as employee 1 (9:00-10:00)
   - Create booking as employee 2 (10:00-11:00)
   - Verify 10-minute buffer inserted
   - Second booking adjusted to 10:10-11:10

4. **Test Waitlist**
   - Book all available slots for a room/day
   - Try to book again
   - Click "Join Waitlist"
   - Verify entry in waitlist panel

5. **Test Slot Request**
   - As employee 1, request someone's booking
   - As employee 2, accept request
   - Verify booking transferred

6. **Test Email Notifications**
   - Check server logs for email sending
   - Verify notifications in inbox

## Customization

### Change TSP Buffer Duration
```
In BookingService.java:
private static final int TSP_BUFFER_MINUTES = 10; // Change to desired value
```

### Modify Room Names
```
In init.sql:
INSERT INTO rooms (name, capacity, description) VALUES
('Room A', 10, ...),
('Room B', 8, ...),
('Room C', 6, ...);
```

### Add More User Roles
```
In User.java model:
public enum UserRole {
    EMPLOYEE, TSP_ADMIN, FACILITY_MANAGER // Add new role
}
```

### Customize Email Templates
```
In EmailService.java:
private String buildBookingConfirmationBody(Booking booking) {
    // Customize email content here
}
```

## Production Deployment

### Prerequisites
- Docker & Docker Compose
- PostgreSQL 15+
- Java 17+ (for local development)
- Node.js 18+ (for local frontend development)

### Steps

1. **Configure Environment**
   ```bash
   cp .env.example .env
   # Edit .env with production values
   ```

2. **Build Images**
   ```bash
   docker-compose build
   ```

3. **Start Services**
   ```bash
   docker-compose up -d
   ```

4. **Verify Services**
   ```bash
   curl http://localhost:8080/api/rooms
   curl http://localhost:3000
   ```

5. **Setup SSL** (if behind reverse proxy)
   - Configure Nginx/Apache for SSL
   - Update CORS origins
   - Update JWT_SECRET

## Troubleshooting

### Build Issues
```bash
# Clear Maven cache
cd backend && mvn clean

# Rebuild
mvn clean install
```

### Database Connection
```bash
# Check PostgreSQL
docker-compose logs postgres

# Verify connection string
# Should be: jdbc:postgresql://postgres:5432/tsp_booking
```

### Frontend Issues
```bash
# Clear npm cache
cd frontend && npm cache clean --force

# Reinstall dependencies
rm -rf node_modules package-lock.json
npm install
```

### API Not Working
```bash
# Check backend logs
docker-compose logs backend | grep -i error

# Verify API URL in frontend
# Should be: http://localhost:8080/api
```

## Next Steps

1. Review code in each file
2. Customize as needed
3. Add additional features
4. Deploy to production
5. Monitor and maintain

## Support

For issues or questions:
1. Check implementation guide files
2. Review Docker logs
3. Check API responses
4. Verify configuration

Version: 1.0.0  
Last Updated: January 2026