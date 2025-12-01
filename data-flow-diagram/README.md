# Data Flow Diagram (DFD) - Airbnb Clone Backend

## Overview

This document describes the Data Flow Diagram for the Airbnb Clone backend system, illustrating how data moves through the system from inputs to processes to outputs.

---

## Level 0: Context Diagram

```
┌─────────────┐                                              ┌─────────────┐
│             │  Registration/Login                          │             │
│    Guest    │ ─────────────────────────────────────────▶  │             │
│             │  Search/Browse                               │             │
│             │ ◀───────────────────────────────────────────  │             │
│             │  Property Results                            │             │
└─────────────┘                                              │             │
                                                             │   AIRBNB    │
┌─────────────┐                                              │    CLONE    │
│             │  Profile/Booking/Payment                     │   SYSTEM    │
│   User/     │ ─────────────────────────────────────────▶  │             │
│   Guest     │  Confirmations/Notifications                 │             │
│             │ ◀───────────────────────────────────────────  │             │
└─────────────┘                                              │             │
                                                             │             │
┌─────────────┐                                              │             │
│             │  Property Listings/Availability              │             │
│    Host     │ ─────────────────────────────────────────▶  │             │
│             │  Bookings/Payouts                            │             │
│             │ ◀───────────────────────────────────────────  │             │
└─────────────┘                                              │             │
                                                             │             │
┌─────────────┐                                              │             │
│   Payment   │  Payment Request                             │             │
│   Gateway   │ ◀───────────────────────────────────────────  │             │
│  (Stripe)   │  Payment Response                            │             │
│             │ ─────────────────────────────────────────▶  │             │
└─────────────┘                                              └─────────────┘
```

---

## Level 1: Main Processes

### Entities

- **Guest/User**: Unregistered or registered users browsing and booking
- **Host**: Users who list properties
- **Admin**: System administrators
- **Payment Gateway**: External payment processor (Stripe/PayPal)
- **Email Service**: External email notification service

### Data Stores

- **D1**: Users Database
- **D2**: Properties Database
- **D3**: Bookings Database
- **D4**: Payments Database
- **D5**: Reviews Database
- **D6**: Messages Database

### Main Processes

```
┌────────────────────────────────────────────────────────────────────────────────────────┐
│                                  AIRBNB CLONE SYSTEM                                    │
│                                                                                        │
│   ┌─────────────────┐                                                                  │
│   │  1.0 User       │◀── User Data ───────────────────────┐                           │
│   │  Authentication │                                      │                           │
│   │  & Management   │────▶ Validated User ─────────────────┼──▶ [D1: Users]           │
│   └─────────────────┘                                      │                           │
│           │                                                │                           │
│           ▼ JWT Token                                      │                           │
│   ┌─────────────────┐                                      │                           │
│   │  2.0 Property   │◀── Property Data ────────────────────┤                           │
│   │  Management     │                                      │                           │
│   │                 │────▶ Listing Info ───────────────────┼──▶ [D2: Properties]      │
│   └─────────────────┘                                      │                           │
│           │                                                │                           │
│           ▼ Available Properties                           │                           │
│   ┌─────────────────┐                                      │                           │
│   │  3.0 Search &   │◀── Search Query ─────────────────────┤                           │
│   │  Discovery      │                                      │                           │
│   │                 │────▶ Search Results ─────────────────┼──▶ Guest/User            │
│   └─────────────────┘                                      │                           │
│           │                                                │                           │
│           ▼ Property Selection                             │                           │
│   ┌─────────────────┐                                      │                           │
│   │  4.0 Booking    │◀── Booking Request ──────────────────┤                           │
│   │  Management     │                                      │                           │
│   │                 │────▶ Booking Record ─────────────────┼──▶ [D3: Bookings]        │
│   └─────────────────┘                                      │                           │
│           │                                                │                           │
│           ▼ Booking Confirmation                           │                           │
│   ┌─────────────────┐                                      │                           │
│   │  5.0 Payment    │◀── Payment Info ─────────────────────┤                           │
│   │  Processing     │                                      │                           │
│   │                 │────▶ Transaction ────────────────────┼──▶ [D4: Payments]        │
│   │                 │◀───────────────────── Payment Gateway                            │
│   └─────────────────┘                                      │                           │
│           │                                                │                           │
│           ▼ Payment Status                                 │                           │
│   ┌─────────────────┐                                      │                           │
│   │  6.0 Review &   │◀── Review Data ──────────────────────┤                           │
│   │  Rating System  │                                      │                           │
│   │                 │────▶ Review Record ──────────────────┼──▶ [D5: Reviews]         │
│   └─────────────────┘                                      │                           │
│           │                                                │                           │
│           ▼ Rating Updates                                 │                           │
│   ┌─────────────────┐                                      │                           │
│   │  7.0 Messaging  │◀── Message Data ─────────────────────┤                           │
│   │  & Notification │                                      │                           │
│   │                 │────▶ Message Record ─────────────────┼──▶ [D6: Messages]        │
│   └─────────────────┘                                      │                           │
│                                                                                        │
└────────────────────────────────────────────────────────────────────────────────────────┘
```

---

## Level 2: Detailed Process Flows

### 2.1 User Authentication Process (Process 1.0)

```
                    ┌────────────────────────────────────────┐
                    │       1.0 USER AUTHENTICATION          │
                    │                                        │
  User Credentials  │  ┌─────────────┐    ┌─────────────┐   │  User Record
  ─────────────────▶│  │ 1.1 Validate │───▶│ 1.2 Create  │──▶│─────────────▶ [D1: Users]
                    │  │ Input       │    │ Account     │   │
                    │  └─────────────┘    └─────────────┘   │
                    │         │                  │           │
                    │         ▼                  ▼           │
                    │  ┌─────────────┐    ┌─────────────┐   │  JWT Token
                    │  │ 1.3 Hash    │───▶│ 1.4 Generate│──▶│─────────────▶ User
                    │  │ Password    │    │ JWT Token   │   │
                    │  └─────────────┘    └─────────────┘   │
                    │                                        │
                    └────────────────────────────────────────┘
```

### 2.2 Property Management Process (Process 2.0)

```
                    ┌────────────────────────────────────────┐
                    │       2.0 PROPERTY MANAGEMENT          │
                    │                                        │
  Property Data     │  ┌─────────────┐    ┌─────────────┐   │  Listing
  ─────────────────▶│  │ 2.1 Validate │───▶│ 2.2 Process │──▶│─────────────▶ [D2: Properties]
  (Host)            │  │ Listing     │    │ Images      │   │
                    │  └─────────────┘    └─────────────┘   │
                    │         │                  │           │
                    │         ▼                  ▼           │
                    │  ┌─────────────┐    ┌─────────────┐   │  Availability
                    │  │ 2.3 Set     │───▶│ 2.4 Publish │──▶│─────────────▶ Search Index
                    │  │ Pricing     │    │ Listing     │   │
                    │  └─────────────┘    └─────────────┘   │
                    │                                        │
                    └────────────────────────────────────────┘
```

### 2.3 Booking Process (Process 4.0)

```
                    ┌────────────────────────────────────────┐
                    │       4.0 BOOKING MANAGEMENT           │
                    │                                        │
  Booking Request   │  ┌─────────────┐    ┌─────────────┐   │
  ─────────────────▶│  │ 4.1 Check   │───▶│ 4.2 Calculate│  │
  (User)            │  │ Availability │    │ Price       │   │
                    │  └─────────────┘    └─────────────┘   │
                    │         │                  │           │
                    │         ▼                  ▼           │  Booking Record
                    │  ┌─────────────┐    ┌─────────────┐   │─────────────▶ [D3: Bookings]
                    │  │ 4.3 Reserve │───▶│ 4.4 Create  │──▶│
                    │  │ Dates       │    │ Booking     │   │  Notification
                    │  └─────────────┘    └─────────────┘   │─────────────▶ Host/User
                    │                                        │
                    └────────────────────────────────────────┘
```

### 2.4 Payment Process (Process 5.0)

```
                    ┌────────────────────────────────────────┐
                    │       5.0 PAYMENT PROCESSING           │
                    │                                        │
  Payment Info      │  ┌─────────────┐    ┌─────────────┐   │  Payment Request
  ─────────────────▶│  │ 5.1 Validate│───▶│ 5.2 Process │──▶│─────────────▶ Stripe/PayPal
  (User)            │  │ Payment     │    │ Payment     │   │
                    │  └─────────────┘    └─────────────┘   │
                    │         │                  │           │
                    │         ▼                  ▼           │  Transaction
                    │  ┌─────────────┐    ┌─────────────┐   │─────────────▶ [D4: Payments]
                    │  │ 5.3 Update  │───▶│ 5.4 Schedule│──▶│
                    │  │ Booking     │    │ Payout      │   │  Receipt
                    │  └─────────────┘    └─────────────┘   │─────────────▶ User
                    │                                        │
                    └────────────────────────────────────────┘
```

---

## Data Dictionary

### User Data

| Field         | Type     | Description            |
| ------------- | -------- | ---------------------- |
| user_id       | UUID     | Unique identifier      |
| email         | String   | User email address     |
| password_hash | String   | Encrypted password     |
| first_name    | String   | User's first name      |
| last_name     | String   | User's last name       |
| phone         | String   | Contact number         |
| role          | Enum     | guest, host, admin     |
| created_at    | DateTime | Registration timestamp |

### Property Data

| Field           | Type    | Description             |
| --------------- | ------- | ----------------------- |
| property_id     | UUID    | Unique identifier       |
| host_id         | UUID    | Reference to host       |
| title           | String  | Property title          |
| description     | Text    | Full description        |
| location        | JSON    | Address, coordinates    |
| price_per_night | Decimal | Nightly rate            |
| max_guests      | Integer | Maximum occupancy       |
| amenities       | Array   | List of amenities       |
| status          | Enum    | draft, active, inactive |

### Booking Data

| Field       | Type    | Description                   |
| ----------- | ------- | ----------------------------- |
| booking_id  | UUID    | Unique identifier             |
| property_id | UUID    | Reference to property         |
| user_id     | UUID    | Reference to guest            |
| check_in    | Date    | Check-in date                 |
| check_out   | Date    | Check-out date                |
| total_price | Decimal | Total booking cost            |
| status      | Enum    | pending, confirmed, cancelled |

### Payment Data

| Field          | Type    | Description                  |
| -------------- | ------- | ---------------------------- |
| payment_id     | UUID    | Unique identifier            |
| booking_id     | UUID    | Reference to booking         |
| amount         | Decimal | Payment amount               |
| currency       | String  | Currency code                |
| status         | Enum    | pending, completed, refunded |
| payment_method | String  | Card, PayPal, etc.           |

---

## Diagram File

The visual Data Flow Diagram created in Draw.io can be found as `data-flow.png` in this directory.

_Note: Create the PNG diagram using Draw.io and export it to this directory._
