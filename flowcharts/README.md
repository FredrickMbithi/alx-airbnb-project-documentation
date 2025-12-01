# Flowcharts - Airbnb Clone Backend

## Overview
This directory contains flowcharts that visualize the step-by-step processes for key backend operations in the Airbnb Clone system.

---

## Property Booking Process Flowchart

This flowchart illustrates the complete booking process from property search to booking confirmation.

```
                                    ┌─────────────────┐
                                    │     START       │
                                    └────────┬────────┘
                                             │
                                             ▼
                                    ┌─────────────────┐
                                    │  User Searches  │
                                    │   Properties    │
                                    └────────┬────────┘
                                             │
                                             ▼
                                    ┌─────────────────┐
                                    │ Display Search  │
                                    │    Results      │
                                    └────────┬────────┘
                                             │
                                             ▼
                                    ┌─────────────────┐
                                    │  User Selects   │
                                    │   Property      │
                                    └────────┬────────┘
                                             │
                                             ▼
                                    ┌─────────────────┐
                                    │ Display Property│
                                    │    Details      │
                                    └────────┬────────┘
                                             │
                                             ▼
                                    ┌─────────────────┐
                                    │  User Selects   │
                                    │  Dates & Guests │
                                    └────────┬────────┘
                                             │
                                             ▼
                               ┌─────────────────────────────┐
                               │                             │
                               │  Check Availability         │
                               │                             │
                               └──────────────┬──────────────┘
                                              │
                                              ▼
                                    ┌─────────────────┐
                                    │   Available?    │
                                    └────────┬────────┘
                                             │
                           ┌─────────────────┼─────────────────┐
                           │ NO              │              YES│
                           ▼                 │                 ▼
              ┌─────────────────┐            │     ┌─────────────────┐
              │  Show Error:    │            │     │ Calculate Total │
              │ "Dates Not      │            │     │     Price       │
              │  Available"     │            │     └────────┬────────┘
              └────────┬────────┘            │              │
                       │                     │              ▼
                       │                     │     ┌─────────────────┐
                       └─────────────────────┘     │ Display Booking │
                                                   │    Summary      │
                                                   └────────┬────────┘
                                                            │
                                                            ▼
                                                   ┌─────────────────┐
                                                   │ User Logged In? │
                                                   └────────┬────────┘
                                                            │
                                      ┌─────────────────────┼─────────────────────┐
                                      │ NO                  │                  YES│
                                      ▼                     │                     ▼
                         ┌─────────────────┐                │        ┌─────────────────┐
                         │  Redirect to    │                │        │  Confirm        │
                         │  Login/Register │                │        │  Booking?       │
                         └────────┬────────┘                │        └────────┬────────┘
                                  │                         │                 │
                                  ▼                         │    ┌────────────┼────────────┐
                         ┌─────────────────┐                │    │ NO         │         YES│
                         │ Login/Register  │                │    ▼            │            ▼
                         │   Successful?   │                │ ┌──────────┐    │  ┌─────────────────┐
                         └────────┬────────┘                │ │  Cancel  │    │  │  Create Booking │
                                  │                         │ │  Process │    │  │  (Pending)      │
                    ┌─────────────┼─────────────┐          │ └──────────┘    │  └────────┬────────┘
                    │ NO          │          YES│          │                 │           │
                    ▼             │             ▼          │                 │           ▼
          ┌─────────────┐        │    ┌─────────────────┐  │                 │  ┌─────────────────┐
          │   END       │        │    │ Return to       │  │                 │  │ Redirect to     │
          │(Auth Failed)│        │    │ Booking Flow    │──┘                 │  │ Payment         │
          └─────────────┘        │    └─────────────────┘                    │  └────────┬────────┘
                                 │                                           │           │
                                 └───────────────────────────────────────────┘           ▼
                                                                               ┌─────────────────┐
                                                                               │  Enter Payment  │
                                                                               │   Details       │
                                                                               └────────┬────────┘
                                                                                        │
                                                                                        ▼
                                                                               ┌─────────────────┐
                                                                               │ Process Payment │
                                                                               └────────┬────────┘
                                                                                        │
                                                                                        ▼
                                                                               ┌─────────────────┐
                                                                               │  Payment        │
                                                                               │  Successful?    │
                                                                               └────────┬────────┘
                                                                                        │
                                                         ┌──────────────────────────────┼──────────────────────────────┐
                                                         │ NO                           │                           YES│
                                                         ▼                              │                              ▼
                                            ┌─────────────────┐                         │                 ┌─────────────────┐
                                            │  Show Error:    │                         │                 │ Update Booking  │
                                            │ "Payment Failed"│                         │                 │ to "Confirmed"  │
                                            └────────┬────────┘                         │                 └────────┬────────┘
                                                     │                                  │                          │
                                                     ▼                                  │                          ▼
                                            ┌─────────────────┐                         │                 ┌─────────────────┐
                                            │  Retry Payment? │                         │                 │ Block Calendar  │
                                            └────────┬────────┘                         │                 │    Dates        │
                                                     │                                  │                 └────────┬────────┘
                                   ┌─────────────────┼─────────────────┐               │                          │
                                   │ YES             │              NO │               │                          ▼
                                   │                 │                 ▼               │                 ┌─────────────────┐
                                   │                 │    ┌─────────────────┐          │                 │ Send Confirm    │
                                   │                 │    │ Cancel Booking  │          │                 │ Email to User   │
                                   │                 │    │ (Release dates) │          │                 └────────┬────────┘
                                   │                 │    └────────┬────────┘          │                          │
                                   │                 │             │                   │                          ▼
                                   │                 │             ▼                   │                 ┌─────────────────┐
                                   │                 │       ┌──────────┐              │                 │ Notify Host of  │
                                   └─────────────────┘       │   END    │              │                 │ New Booking     │
                                                             └──────────┘              │                 └────────┬────────┘
                                                                                       │                          │
                                                                                       │                          ▼
                                                                                       │                 ┌─────────────────┐
                                                                                       │                 │ Display Booking │
                                                                                       │                 │ Confirmation    │
                                                                                       │                 └────────┬────────┘
                                                                                       │                          │
                                                                                       │                          ▼
                                                                                       │                    ┌──────────┐
                                                                                       └────────────────────│   END    │
                                                                                                            │(Success) │
                                                                                                            └──────────┘
```

---

## User Registration Flowchart

```
                                    ┌─────────────────┐
                                    │     START       │
                                    └────────┬────────┘
                                             │
                                             ▼
                                    ┌─────────────────┐
                                    │ User Clicks     │
                                    │ "Register"      │
                                    └────────┬────────┘
                                             │
                                             ▼
                                    ┌─────────────────┐
                                    │ Display         │
                                    │ Registration    │
                                    │ Form            │
                                    └────────┬────────┘
                                             │
                                             ▼
                                    ┌─────────────────┐
                                    │ User Enters:    │
                                    │ - Email         │
                                    │ - Password      │
                                    │ - Name          │
                                    └────────┬────────┘
                                             │
                                             ▼
                                    ┌─────────────────┐
                                    │ Validate Input  │
                                    └────────┬────────┘
                                             │
                                             ▼
                                    ┌─────────────────┐
                                    │ Input Valid?    │
                                    └────────┬────────┘
                                             │
                           ┌─────────────────┼─────────────────┐
                           │ NO              │              YES│
                           ▼                 │                 ▼
              ┌─────────────────┐            │     ┌─────────────────┐
              │ Show Validation │            │     │ Check if Email  │
              │ Errors          │            │     │ Already Exists  │
              └────────┬────────┘            │     └────────┬────────┘
                       │                     │              │
                       └─────────────────────┘              ▼
                                                   ┌─────────────────┐
                                                   │ Email Exists?   │
                                                   └────────┬────────┘
                                                            │
                                      ┌─────────────────────┼─────────────────────┐
                                      │ YES                 │                  NO │
                                      ▼                     │                     ▼
                         ┌─────────────────┐                │        ┌─────────────────┐
                         │ Show Error:     │                │        │ Hash Password   │
                         │ "Email Already  │                │        │                 │
                         │ Registered"     │                │        └────────┬────────┘
                         └─────────────────┘                │                 │
                                                            │                 ▼
                                                            │        ┌─────────────────┐
                                                            │        │ Create User     │
                                                            │        │ Record in DB    │
                                                            │        └────────┬────────┘
                                                            │                 │
                                                            │                 ▼
                                                            │        ┌─────────────────┐
                                                            │        │ Generate Email  │
                                                            │        │ Verification    │
                                                            │        │ Token           │
                                                            │        └────────┬────────┘
                                                            │                 │
                                                            │                 ▼
                                                            │        ┌─────────────────┐
                                                            │        │ Send Verification│
                                                            │        │ Email            │
                                                            │        └────────┬────────┘
                                                            │                 │
                                                            │                 ▼
                                                            │        ┌─────────────────┐
                                                            │        │ Display Success │
                                                            │        │ "Check Your     │
                                                            │        │  Email"         │
                                                            │        └────────┬────────┘
                                                            │                 │
                                                            │                 ▼
                                                            │           ┌──────────┐
                                                            │           │   END    │
                                                            └───────────└──────────┘
```

---

## Diagram Files
The visual flowcharts created in Draw.io should be stored in this directory:
- `data-flow-diagram.png` - Main booking process flowchart

*Note: Create the PNG diagrams using Draw.io and export them to this directory.*
