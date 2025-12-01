# Use Case Diagram - Airbnb Clone Backend

## Overview

This document describes the use case diagram for the Airbnb Clone application, illustrating the interactions between different actors and the system.

## Actors

### 1. Guest (Unregistered User)

- Can browse properties
- Can search for listings
- Can view property details
- Can register for an account

### 2. Registered User (Guest Role)

- All Guest capabilities
- Can log in/log out
- Can manage profile
- Can book properties
- Can make payments
- Can leave reviews
- Can message hosts
- Can view booking history

### 3. Host

- All Registered User capabilities
- Can list properties
- Can manage property listings
- Can set availability
- Can respond to bookings
- Can receive payouts
- Can respond to reviews

### 4. Admin

- Can manage all users
- Can manage all properties
- Can view analytics
- Can handle disputes
- Can configure system settings

---

## Use Cases

### User Management Use Cases

| Use Case         | Actor(s)                     | Description                                            |
| ---------------- | ---------------------------- | ------------------------------------------------------ |
| Register Account | Guest                        | Create a new user account with email/password or OAuth |
| Login            | Registered User, Host, Admin | Authenticate and access the system                     |
| Logout           | Registered User, Host, Admin | End session and log out                                |
| Update Profile   | Registered User, Host        | Modify personal information                            |
| Reset Password   | Registered User, Host        | Reset forgotten password via email                     |
| Delete Account   | Registered User, Host        | Permanently delete user account                        |

### Property Management Use Cases

| Use Case              | Actor(s)               | Description                            |
| --------------------- | ---------------------- | -------------------------------------- |
| Browse Properties     | Guest, Registered User | View available property listings       |
| Search Properties     | Guest, Registered User | Search using location, dates, guests   |
| Filter Properties     | Guest, Registered User | Apply filters (price, amenities, etc.) |
| View Property Details | Guest, Registered User | See full property information          |
| Create Listing        | Host                   | Add a new property listing             |
| Update Listing        | Host                   | Modify property details                |
| Delete Listing        | Host                   | Remove a property listing              |
| Manage Availability   | Host                   | Set available dates and pricing        |

### Booking Use Cases

| Use Case           | Actor(s)              | Description                            |
| ------------------ | --------------------- | -------------------------------------- |
| Check Availability | Registered User       | Verify property availability for dates |
| Create Booking     | Registered User       | Book a property for specific dates     |
| View Booking       | Registered User, Host | See booking details                    |
| Cancel Booking     | Registered User, Host | Cancel an existing booking             |
| Approve Booking    | Host                  | Accept a booking request               |
| Decline Booking    | Host                  | Reject a booking request               |

### Payment Use Cases

| Use Case             | Actor(s)              | Description                        |
| -------------------- | --------------------- | ---------------------------------- |
| Make Payment         | Registered User       | Pay for a booking                  |
| Process Refund       | System, Admin         | Refund cancelled bookings          |
| Receive Payout       | Host                  | Get payment for completed bookings |
| View Payment History | Registered User, Host | See transaction history            |

### Review Use Cases

| Use Case          | Actor(s)               | Description                  |
| ----------------- | ---------------------- | ---------------------------- |
| Leave Review      | Registered User        | Review a property after stay |
| Respond to Review | Host                   | Reply to guest reviews       |
| View Reviews      | Guest, Registered User | Read property reviews        |
| Report Review     | Registered User, Host  | Flag inappropriate content   |

### Communication Use Cases

| Use Case             | Actor(s)              | Description                  |
| -------------------- | --------------------- | ---------------------------- |
| Send Message         | Registered User, Host | Communicate with other users |
| View Messages        | Registered User, Host | Read conversation history    |
| Receive Notification | Registered User, Host | Get alerts for activities    |

### Admin Use Cases

| Use Case        | Actor(s) | Description                    |
| --------------- | -------- | ------------------------------ |
| Manage Users    | Admin    | View, suspend, or delete users |
| Manage Listings | Admin    | Approve or remove properties   |
| View Analytics  | Admin    | Access system statistics       |
| Handle Disputes | Admin    | Resolve user conflicts         |

---

## Use Case Diagram

```
                              ┌─────────────────────────────────────────────────────────────┐
                              │                    AIRBNB CLONE SYSTEM                       │
                              │                                                              │
    ┌───────┐                │  ┌──────────────────┐    ┌──────────────────┐               │
    │ Guest │────────────────┼──│  Browse Properties │    │ Search Properties │               │
    └───────┘                │  └──────────────────┘    └──────────────────┘               │
        │                    │           │                       │                          │
        │                    │  ┌──────────────────┐    ┌──────────────────┐               │
        │                    │  │ View Property     │    │ Register Account │               │
        │                    │  │ Details           │    └──────────────────┘               │
        │                    │  └──────────────────┘             │                          │
        │                    │                                   │                          │
        ▼                    │                                   ▼                          │
    ┌────────────┐          │  ┌──────────────────┐    ┌──────────────────┐               │
    │ Registered │──────────┼──│ Login/Logout     │    │ Update Profile   │               │
    │ User       │          │  └──────────────────┘    └──────────────────┘               │
    └────────────┘          │                                                              │
        │                    │  ┌──────────────────┐    ┌──────────────────┐               │
        │                    │  │ Create Booking   │    │ Make Payment     │               │
        │                    │  └──────────────────┘    └──────────────────┘               │
        │                    │                                                              │
        │                    │  ┌──────────────────┐    ┌──────────────────┐               │
        │                    │  │ Leave Review     │    │ Send Message     │               │
        │                    │  └──────────────────┘    └──────────────────┘               │
        │                    │                                                              │
        ▼                    │                                                              │
    ┌───────┐               │  ┌──────────────────┐    ┌──────────────────┐               │
    │ Host  │───────────────┼──│ Create Listing   │    │ Manage Listings  │               │
    └───────┘               │  └──────────────────┘    └──────────────────┘               │
        │                    │                                                              │
        │                    │  ┌──────────────────┐    ┌──────────────────┐               │
        │                    │  │ Approve Booking  │    │ Receive Payout   │               │
        │                    │  └──────────────────┘    └──────────────────┘               │
        │                    │                                                              │
        ▼                    │  ┌──────────────────┐    ┌──────────────────┐               │
    ┌───────┐               │  │ Manage Users     │    │ Handle Disputes  │               │
    │ Admin │───────────────┼──└──────────────────┘    └──────────────────┘               │
    └───────┘               │                                                              │
                              │  ┌──────────────────┐    ┌──────────────────┐               │
                              │  │ View Analytics   │    │ Manage Listings  │               │
                              │  └──────────────────┘    │ (Admin)          │               │
                              │                          └──────────────────┘               │
                              └─────────────────────────────────────────────────────────────┘
```

---

## Diagram File

The visual use case diagram created in Draw.io can be found as `use-case-diagram.png` in this directory.

_Note: Create the PNG diagram using Draw.io and export it to this directory._
