# 🏡 Airbnb Clone Backend — Detailed Feature Specifications

## ✨ 1️⃣ User Registration & Account Management

### 📄 Description

Allow guests and hosts to register, log in, update profiles, and manage their account information securely.

### 🌐 API Endpoints

#### `POST /api/users/register/`

- **Input:**
  - `first_name`: string, required, max 20 characters
  - `last_name`: string, required, max 20 characters
  - `email`: string, required, must be a valid email, max 20
  - `password`: string, required, min 8 characters
  - `role`: string, required, enum: `guest`, `host`, `admin`
- **Output:**
  - `201 Created` with user ID and success message
  - `400 Bad Request` with validation errors

#### `POST /api/users/login/`

- **Input:**
  - `email`: string, required
  - `password`: string, required
- **Output:**
  - `200 OK` with authentication token
  - `401 Unauthorized` for invalid credentials

#### `PATCH /api/users/profile/`

- **Input:**
  - `first_name`, `last_name`, `phone_number`, `profile_picture` (optional fields)
- **Output:**
  - `200 OK` with updated user data
  - `400 Bad Request` if validation fails

### ✅ Validation Rules

- Email must be unique.
- Password must meet complexity requirements (at least one number and one uppercase letter recommended).
- Role must be either `guest`, `host` or `admin`.

### ⚡ Performance Criteria

- Registration and login responses should fast under normal load.
- Token generation and validation must be secure and optimized.

---

## 🏠 2️⃣ Property Management

### 📄 Description

Enable hosts to create, update, delete, and manage their property listings.

### 🌐 API Endpoints

#### `POST /api/properties/`

- **Input:**
  - `host_id`: string, required, max 36 characters
  - `name`: string, required, max 20 characters
  - `description`: string, required
  - `location`: string, required, max 50 characters
  - `pricepernight`: decimal, required, positive
- **Output:**
  - `201 Created` with property ID and details
  - `400 Bad Request` for validation errors

#### `PATCH /api/properties/{id}/`

- **Input:**
  - Any updatable fields (e.g., name, description, price, location)
- **Output:**
  - `200 OK` with updated property details
  - `404 Not Found` if property does not exist

#### `DELETE /api/properties/{id}/`

- **Output:**
  - `204 No Content` on success
  - `404 Not Found` if property does not exist

### ✅ Validation Rules

- Only authenticated hosts can create or update properties.
- Price must be a positive number.
- Location must not be empty.

### ⚡ Performance Criteria

- Listing creation should be fast.

---

## 📅 3️⃣ Booking & Payment Processing

### 📄 Description

Allow guests to book properties and securely process payments.

### 🌐 API Endpoints

#### `POST /api/v1/bookings/`

- **Input:**
  - `property_id`: UUID, required
  - `start_date`: date, required
  - `end_date`: date, required
- **Output:**
  - `201 Created` with booking ID and details
  - `400 Bad Request` for validation errors

#### `POST /api/payments/`

- **Input:**
  - `booking_id`: UUID, required
  - `payment_method`: string, required, enum: `credit_card`, `paypal`, `stripe`
  - `amount`: decimal, required, should match booking total
- **Output:**
  - `200 OK` with payment confirmation
  - `400 Bad Request` for failed payment

### ✅ Validation Rules

- Check-in date must be before check-out date.
- Payment amount must match the booking cost exactly.

### ⚡ Performance Criteria

- Booking creation and availability checks should be fast.
- Payment confirmation should return shortly.
- Ensure strong concurrency handling to prevent double bookings.

---
