# User Stories - Airbnb Clone Backend

## Overview

This document contains user stories derived from the use case diagram, capturing the core interactions between users and the Airbnb Clone system.

---

## User Authentication Stories

### 1. User Registration

**As a** guest,  
**I want to** register an account using my email and password,  
**So that** I can book properties and list my own properties on the platform.

**Acceptance Criteria:**

- User can register with email, password, first name, and last name
- Email must be unique and valid format
- Password must meet security requirements (min 8 characters, uppercase, lowercase, number)
- User receives email verification link
- User cannot access protected features until email is verified

---

### 2. User Login

**As a** registered user,  
**I want to** log in to my account securely,  
**So that** I can access my bookings, listings, and personal information.

**Acceptance Criteria:**

- User can log in with email and password
- System provides JWT token upon successful authentication
- User sees appropriate error message for invalid credentials
- Session remains active based on token expiration
- User can use OAuth (Google, Facebook) for quick login

---

### 3. Profile Management

**As a** registered user,  
**I want to** update my profile information,  
**So that** my account reflects my current details and preferences.

**Acceptance Criteria:**

- User can update name, phone number, and profile photo
- User can change password with current password verification
- User can add or update payment methods
- Changes are saved immediately and reflected across the platform

---

## Property Management Stories

### 4. Property Listing Creation

**As a** host,  
**I want to** create a property listing with details and photos,  
**So that** potential guests can find and book my property.

**Acceptance Criteria:**

- Host can add title, description, and property type
- Host can specify location with address and map coordinates
- Host can upload multiple photos (minimum 5)
- Host can set price per night and additional fees
- Host can list amenities and house rules
- Host can set guest capacity (guests, bedrooms, bathrooms)
- Listing is saved as draft until published

---

### 5. Property Search

**As a** user,  
**I want to** search for properties by location, dates, and number of guests,  
**So that** I can find suitable accommodation for my trip.

**Acceptance Criteria:**

- User can search by city, country, or specific address
- User can specify check-in and check-out dates
- User can specify number of guests
- Results show only available properties
- Results display price, rating, and key details
- Pagination is implemented for large result sets

---

### 6. Property Filtering

**As a** user,  
**I want to** filter search results by price range, amenities, and property type,  
**So that** I can narrow down options to match my preferences.

**Acceptance Criteria:**

- User can filter by minimum and maximum price
- User can filter by property type (apartment, house, villa, etc.)
- User can filter by amenities (Wi-Fi, pool, parking, etc.)
- User can filter by rating (4+ stars, etc.)
- Filters can be combined
- Results update dynamically as filters are applied

---

## Booking Stories

### 7. Property Booking

**As a** registered user,  
**I want to** book a property for specific dates,  
**So that** I can secure accommodation for my trip.

**Acceptance Criteria:**

- User can select check-in and check-out dates
- System validates property availability for selected dates
- System calculates total price including fees
- User can review booking details before confirming
- Booking is created with "pending" status
- User receives booking confirmation email

---

### 8. Booking Cancellation

**As a** registered user,  
**I want to** cancel my booking,  
**So that** I can change my travel plans if needed.

**Acceptance Criteria:**

- User can cancel booking from booking details page
- System shows cancellation policy and refund amount
- User confirms cancellation action
- Booking status changes to "cancelled"
- Refund is processed according to cancellation policy
- Both guest and host receive notification emails

---

### 9. Booking Management (Host)

**As a** host,  
**I want to** view and manage booking requests for my properties,  
**So that** I can accept or decline reservations.

**Acceptance Criteria:**

- Host receives notification for new booking requests
- Host can view guest profile and booking details
- Host can approve or decline pending bookings
- Host can message guest before making a decision
- Guest is notified of host's decision

---

## Payment Stories

### 10. Booking Payment

**As a** registered user,  
**I want to** pay for my booking securely,  
**So that** my reservation is confirmed.

**Acceptance Criteria:**

- User can pay using credit/debit card
- Payment is processed securely (Stripe/PayPal integration)
- User receives payment confirmation
- Booking status changes to "confirmed" after payment
- Payment details are stored securely for future use (optional)

---

### 11. Host Payout

**As a** host,  
**I want to** receive payouts for completed bookings,  
**So that** I can earn income from my property.

**Acceptance Criteria:**

- Host can add payout methods (bank account, PayPal)
- Payout is initiated after guest check-in (24-48 hours)
- Host can view payout history and pending payouts
- Host receives notification when payout is processed
- Platform fee is deducted before payout

---

## Review Stories

### 12. Leave a Review

**As a** registered user,  
**I want to** leave a review after my stay,  
**So that** I can share my experience with other users.

**Acceptance Criteria:**

- Review option is available after checkout date
- User can rate property (1-5 stars)
- User can write text review (min 10 characters)
- Review is published immediately
- Review window expires after 14 days
- User can only review properties they have booked

---

### 13. Host Review Response

**As a** host,  
**I want to** respond to guest reviews,  
**So that** I can address feedback and engage with guests.

**Acceptance Criteria:**

- Host receives notification when review is posted
- Host can write one response per review
- Response appears below the original review
- Host cannot edit or delete response after posting

---

## Communication Stories

### 14. Messaging

**As a** registered user,  
**I want to** message hosts directly,  
**So that** I can ask questions about properties before booking.

**Acceptance Criteria:**

- User can send messages from property page
- Messages are delivered in real-time
- Both parties receive email notifications
- Message history is preserved
- Users can attach images to messages

---

### 15. Notifications

**As a** registered user,  
**I want to** receive notifications for important activities,  
**So that** I stay informed about my bookings and messages.

**Acceptance Criteria:**

- User receives notifications for new messages
- User receives notifications for booking status changes
- User receives notifications for new reviews
- Notifications appear in-app and via email
- User can customize notification preferences

---

## Summary

These user stories cover the core functionality of the Airbnb Clone backend, including:

- User authentication and profile management
- Property listing and search
- Booking workflow
- Payment processing
- Review system
- Messaging and notifications

Each story follows the standard format: **As a** [role], **I want to** [action], **So that** [benefit], with clear acceptance criteria for implementation.
