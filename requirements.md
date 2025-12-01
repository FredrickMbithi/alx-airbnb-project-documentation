# Backend Requirement Specifications - Airbnb Clone

## Overview

This document outlines the detailed technical and functional requirements for the key backend features of the Airbnb Clone application.

---

## 1. User Authentication

### 1.1 Description

The User Authentication system handles user registration, login, logout, and session management using JWT tokens.

### 1.2 API Endpoints

| Method | Endpoint                        | Description                      |
| ------ | ------------------------------- | -------------------------------- |
| POST   | `/api/auth/register`            | Register a new user              |
| POST   | `/api/auth/login`               | Authenticate user and return JWT |
| POST   | `/api/auth/logout`              | Invalidate user session          |
| POST   | `/api/auth/refresh`             | Refresh JWT token                |
| POST   | `/api/auth/forgot-password`     | Initiate password reset          |
| POST   | `/api/auth/reset-password`      | Reset password with token        |
| GET    | `/api/auth/verify-email/:token` | Verify email address             |

### 1.3 Input/Output Specifications

#### POST /api/auth/register

**Request Body:**

```json
{
  "email": "user@example.com",
  "password": "SecurePass123!",
  "first_name": "John",
  "last_name": "Doe",
  "phone_number": "+1234567890"
}
```

**Success Response (201 Created):**

```json
{
  "status": "success",
  "message": "Registration successful. Please verify your email.",
  "data": {
    "user_id": "uuid-string",
    "email": "user@example.com",
    "first_name": "John",
    "last_name": "Doe",
    "created_at": "2025-12-01T10:00:00Z"
  }
}
```

**Error Response (400 Bad Request):**

```json
{
  "status": "error",
  "message": "Validation failed",
  "errors": [
    {
      "field": "email",
      "message": "Email already registered"
    }
  ]
}
```

#### POST /api/auth/login

**Request Body:**

```json
{
  "email": "user@example.com",
  "password": "SecurePass123!"
}
```

**Success Response (200 OK):**

```json
{
  "status": "success",
  "data": {
    "access_token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
    "refresh_token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
    "expires_in": 3600,
    "user": {
      "user_id": "uuid-string",
      "email": "user@example.com",
      "first_name": "John",
      "last_name": "Doe",
      "role": "guest"
    }
  }
}
```

### 1.4 Validation Rules

| Field        | Rules                                                                                    |
| ------------ | ---------------------------------------------------------------------------------------- |
| email        | Required, valid email format, unique                                                     |
| password     | Required, min 8 characters, must contain uppercase, lowercase, number, special character |
| first_name   | Required, min 2 characters, max 50 characters, letters only                              |
| last_name    | Required, min 2 characters, max 50 characters, letters only                              |
| phone_number | Optional, valid international phone format                                               |

### 1.5 Performance Criteria

- Registration response time: < 500ms
- Login response time: < 300ms
- JWT token expiration: 1 hour (access), 7 days (refresh)
- Password hashing: bcrypt with 12 salt rounds
- Rate limiting: 5 login attempts per minute per IP
- Email verification link expiration: 24 hours

### 1.6 Security Requirements

- Passwords must be hashed using bcrypt
- JWT tokens must be signed with RS256 algorithm
- HTTPS required for all authentication endpoints
- Rate limiting to prevent brute force attacks
- Account lockout after 5 failed login attempts (15 minutes)

---

## 2. Property Management

### 2.1 Description

The Property Management system enables hosts to create, update, and manage their property listings.

### 2.2 API Endpoints

| Method | Endpoint                              | Description                      |
| ------ | ------------------------------------- | -------------------------------- |
| POST   | `/api/properties`                     | Create a new property listing    |
| GET    | `/api/properties`                     | List all properties with filters |
| GET    | `/api/properties/:id`                 | Get property details             |
| PUT    | `/api/properties/:id`                 | Update property                  |
| DELETE | `/api/properties/:id`                 | Delete property                  |
| POST   | `/api/properties/:id/images`          | Upload property images           |
| DELETE | `/api/properties/:id/images/:imageId` | Delete property image            |
| PUT    | `/api/properties/:id/availability`    | Update availability              |

### 2.3 Input/Output Specifications

#### POST /api/properties

**Request Headers:**

```
Authorization: Bearer <access_token>
Content-Type: application/json
```

**Request Body:**

```json
{
  "title": "Cozy Downtown Apartment",
  "description": "A beautiful apartment in the heart of the city...",
  "property_type": "apartment",
  "location": {
    "address": "123 Main Street",
    "city": "New York",
    "state": "NY",
    "country": "USA",
    "zip_code": "10001",
    "latitude": 40.7128,
    "longitude": -74.006
  },
  "price_per_night": 150.0,
  "currency": "USD",
  "max_guests": 4,
  "bedrooms": 2,
  "bathrooms": 1,
  "amenities": ["wifi", "kitchen", "washer", "air_conditioning"],
  "house_rules": ["No smoking", "No parties", "Check-in after 3 PM"],
  "cancellation_policy": "moderate"
}
```

**Success Response (201 Created):**

```json
{
  "status": "success",
  "data": {
    "property_id": "uuid-string",
    "title": "Cozy Downtown Apartment",
    "description": "A beautiful apartment in the heart of the city...",
    "property_type": "apartment",
    "location": {
      "address": "123 Main Street",
      "city": "New York",
      "state": "NY",
      "country": "USA",
      "zip_code": "10001",
      "coordinates": {
        "latitude": 40.7128,
        "longitude": -74.006
      }
    },
    "price_per_night": 150.0,
    "currency": "USD",
    "max_guests": 4,
    "bedrooms": 2,
    "bathrooms": 1,
    "amenities": ["wifi", "kitchen", "washer", "air_conditioning"],
    "house_rules": ["No smoking", "No parties", "Check-in after 3 PM"],
    "cancellation_policy": "moderate",
    "status": "draft",
    "host": {
      "user_id": "uuid-string",
      "first_name": "John",
      "last_name": "Doe"
    },
    "created_at": "2025-12-01T10:00:00Z",
    "updated_at": "2025-12-01T10:00:00Z"
  }
}
```

#### GET /api/properties

**Query Parameters:**

```
?location=New York
&check_in=2025-12-15
&check_out=2025-12-20
&guests=2
&min_price=100
&max_price=300
&property_type=apartment
&amenities=wifi,kitchen
&page=1
&limit=20
&sort_by=price
&sort_order=asc
```

**Success Response (200 OK):**

```json
{
  "status": "success",
  "data": {
    "properties": [
      {
        "property_id": "uuid-string",
        "title": "Cozy Downtown Apartment",
        "property_type": "apartment",
        "location": {
          "city": "New York",
          "country": "USA"
        },
        "price_per_night": 150.0,
        "currency": "USD",
        "rating": 4.8,
        "review_count": 45,
        "thumbnail": "https://example.com/images/property1.jpg",
        "max_guests": 4
      }
    ],
    "pagination": {
      "current_page": 1,
      "total_pages": 5,
      "total_items": 100,
      "items_per_page": 20
    }
  }
}
```

### 2.4 Validation Rules

| Field               | Rules                                                          |
| ------------------- | -------------------------------------------------------------- |
| title               | Required, min 10 characters, max 100 characters                |
| description         | Required, min 50 characters, max 5000 characters               |
| property_type       | Required, enum: apartment, house, villa, condo, cabin, cottage |
| location.address    | Required, max 200 characters                                   |
| location.city       | Required, max 100 characters                                   |
| location.country    | Required, valid country name                                   |
| location.latitude   | Required, valid latitude (-90 to 90)                           |
| location.longitude  | Required, valid longitude (-180 to 180)                        |
| price_per_night     | Required, positive number, min 1                               |
| max_guests          | Required, integer, min 1, max 16                               |
| bedrooms            | Required, integer, min 0, max 10                               |
| bathrooms           | Required, integer, min 1, max 10                               |
| amenities           | Array of valid amenity strings                                 |
| cancellation_policy | Required, enum: flexible, moderate, strict                     |

### 2.5 Performance Criteria

- Property creation: < 1s
- Property search: < 500ms for up to 10,000 properties
- Image upload: < 5s per image
- Maximum images per property: 20
- Maximum image size: 10MB
- Supported image formats: JPEG, PNG, WebP
- Search results pagination: 20 items per page (configurable)

### 2.6 Authorization Rules

- Only authenticated users can create listings
- Only property owner can update/delete their listings
- Admins can manage any listing
- Published properties are visible to all users
- Draft properties are only visible to owner

---

## 3. Booking System

### 3.1 Description

The Booking System manages reservations, availability, and booking status for properties.

### 3.2 API Endpoints

| Method | Endpoint                           | Description                 |
| ------ | ---------------------------------- | --------------------------- |
| POST   | `/api/bookings`                    | Create a new booking        |
| GET    | `/api/bookings`                    | List user's bookings        |
| GET    | `/api/bookings/:id`                | Get booking details         |
| PUT    | `/api/bookings/:id`                | Update booking              |
| DELETE | `/api/bookings/:id/cancel`         | Cancel booking              |
| GET    | `/api/properties/:id/availability` | Check property availability |
| POST   | `/api/bookings/:id/confirm`        | Host confirms booking       |
| POST   | `/api/bookings/:id/decline`        | Host declines booking       |

### 3.3 Input/Output Specifications

#### POST /api/bookings

**Request Headers:**

```
Authorization: Bearer <access_token>
Content-Type: application/json
```

**Request Body:**

```json
{
  "property_id": "uuid-string",
  "check_in": "2025-12-15",
  "check_out": "2025-12-20",
  "guests": {
    "adults": 2,
    "children": 1,
    "infants": 0
  },
  "special_requests": "Early check-in if possible"
}
```

**Success Response (201 Created):**

```json
{
  "status": "success",
  "data": {
    "booking_id": "uuid-string",
    "property": {
      "property_id": "uuid-string",
      "title": "Cozy Downtown Apartment",
      "location": {
        "city": "New York",
        "country": "USA"
      },
      "thumbnail": "https://example.com/images/property1.jpg"
    },
    "host": {
      "user_id": "uuid-string",
      "first_name": "John",
      "last_name": "Doe"
    },
    "guest": {
      "user_id": "uuid-string",
      "first_name": "Jane",
      "last_name": "Smith"
    },
    "check_in": "2025-12-15",
    "check_out": "2025-12-20",
    "nights": 5,
    "guests": {
      "adults": 2,
      "children": 1,
      "infants": 0,
      "total": 3
    },
    "pricing": {
      "price_per_night": 150.0,
      "nights_total": 750.0,
      "cleaning_fee": 50.0,
      "service_fee": 80.0,
      "taxes": 66.0,
      "total": 946.0,
      "currency": "USD"
    },
    "status": "pending",
    "special_requests": "Early check-in if possible",
    "cancellation_policy": "moderate",
    "created_at": "2025-12-01T10:00:00Z",
    "expires_at": "2025-12-02T10:00:00Z"
  }
}
```

#### GET /api/properties/:id/availability

**Query Parameters:**

```
?start_date=2025-12-01
&end_date=2025-12-31
```

**Success Response (200 OK):**

```json
{
  "status": "success",
  "data": {
    "property_id": "uuid-string",
    "available_dates": [
      {
        "date": "2025-12-01",
        "available": true,
        "price": 150.0
      },
      {
        "date": "2025-12-02",
        "available": true,
        "price": 150.0
      },
      {
        "date": "2025-12-15",
        "available": false,
        "reason": "booked"
      }
    ],
    "minimum_stay": 2,
    "maximum_stay": 30
  }
}
```

### 3.4 Validation Rules

| Field            | Rules                                              |
| ---------------- | -------------------------------------------------- |
| property_id      | Required, valid property UUID, property must exist |
| check_in         | Required, valid date, must be today or future      |
| check_out        | Required, valid date, must be after check_in       |
| guests.adults    | Required, integer, min 1                           |
| guests.children  | Optional, integer, min 0                           |
| guests.infants   | Optional, integer, min 0                           |
| Total guests     | Must not exceed property max_guests                |
| Stay duration    | Must be within min/max stay limits                 |
| special_requests | Optional, max 500 characters                       |

### 3.5 Booking Status Flow

```
pending → confirmed → active → completed
pending → cancelled
pending → declined
confirmed → cancelled
```

### 3.6 Performance Criteria

- Booking creation: < 500ms
- Availability check: < 200ms
- Concurrent booking prevention: Optimistic locking
- Booking expiration: 24 hours for pending bookings
- Calendar sync latency: < 30 seconds

### 3.7 Business Rules

- Users cannot book their own properties
- Double booking prevention with database-level constraints
- Cancellation refund based on policy:
  - Flexible: Full refund up to 24 hours before check-in
  - Moderate: Full refund up to 5 days before check-in
  - Strict: 50% refund up to 7 days before check-in
- Automatic booking cancellation if payment not received within 24 hours
- Host has 24 hours to respond to booking request

---

## 4. Payment System

### 4.1 Description

The Payment System handles payment processing, refunds, and host payouts using Stripe integration.

### 4.2 API Endpoints

| Method | Endpoint                   | Description           |
| ------ | -------------------------- | --------------------- |
| POST   | `/api/payments/intent`     | Create payment intent |
| POST   | `/api/payments/confirm`    | Confirm payment       |
| GET    | `/api/payments/:id`        | Get payment details   |
| POST   | `/api/payments/:id/refund` | Process refund        |
| GET    | `/api/payments/history`    | Get payment history   |
| POST   | `/api/payouts/setup`       | Setup payout method   |
| GET    | `/api/payouts`             | Get payout history    |

### 4.3 Input/Output Specifications

#### POST /api/payments/intent

**Request Body:**

```json
{
  "booking_id": "uuid-string"
}
```

**Success Response (200 OK):**

```json
{
  "status": "success",
  "data": {
    "payment_intent_id": "pi_stripe_id",
    "client_secret": "pi_xxx_secret_xxx",
    "amount": 94600,
    "currency": "usd",
    "booking_id": "uuid-string"
  }
}
```

### 4.4 Validation Rules

| Field          | Rules                                                           |
| -------------- | --------------------------------------------------------------- |
| booking_id     | Required, valid booking UUID, booking must be in pending status |
| payment_method | Valid Stripe payment method ID                                  |
| Refund amount  | Cannot exceed original payment amount                           |

### 4.5 Performance Criteria

- Payment intent creation: < 2s
- Payment confirmation: < 5s
- Refund processing: < 10s
- Payout processing: 1-3 business days

### 4.6 Security Requirements

- PCI DSS compliance
- Sensitive data never stored on server
- All payment data handled by Stripe
- Webhook signature verification

---

## Non-Functional Requirements

### Performance

- API response time: p95 < 500ms
- Database query time: p95 < 100ms
- Concurrent users: Support 10,000 concurrent users
- Uptime: 99.9% availability

### Security

- All endpoints use HTTPS
- JWT token-based authentication
- Input sanitization to prevent SQL injection
- XSS protection
- CORS configured for allowed origins
- Rate limiting: 100 requests per minute per user

### Scalability

- Horizontal scaling support
- Database connection pooling
- Redis caching for frequently accessed data
- CDN for static assets

### Monitoring

- Request/response logging
- Error tracking and alerting
- Performance metrics collection
- Health check endpoints

---

## API Response Format

### Success Response

```json
{
  "status": "success",
  "data": { ... },
  "meta": {
    "timestamp": "2025-12-01T10:00:00Z",
    "request_id": "uuid-string"
  }
}
```

### Error Response

```json
{
  "status": "error",
  "message": "Human-readable error message",
  "code": "ERROR_CODE",
  "errors": [
    {
      "field": "field_name",
      "message": "Specific error message"
    }
  ],
  "meta": {
    "timestamp": "2025-12-01T10:00:00Z",
    "request_id": "uuid-string"
  }
}
```

### HTTP Status Codes

| Code | Description                                         |
| ---- | --------------------------------------------------- |
| 200  | OK - Request successful                             |
| 201  | Created - Resource created                          |
| 400  | Bad Request - Invalid input                         |
| 401  | Unauthorized - Authentication required              |
| 403  | Forbidden - Insufficient permissions                |
| 404  | Not Found - Resource not found                      |
| 409  | Conflict - Resource conflict (e.g., double booking) |
| 422  | Unprocessable Entity - Validation error             |
| 429  | Too Many Requests - Rate limit exceeded             |
| 500  | Internal Server Error - Server error                |
