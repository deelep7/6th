# Hotel Booking System - Entity Relationship (ER) Diagram

## ER Diagram Overview

```
┌─────────────────────────────────────────────────────────────────────────────────┐
│                                    USERS                                        │
├─────────────────────────────────────────────────────────────────────────────────┤
│ PK: _id (ObjectId)                                                              │
│     email (String, Unique)                                                      │
│     password (String, Hashed)                                                   │
│     firstName (String)                                                          │
│     lastName (String)                                                           │
│     role (Enum: "user", "hotel owner", "admin")                                │
│     clickedHotels (Array of ObjectId)                                          │
└─────────────────────────────────────────────────────────────────────────────────┘
                                    │
                                    │ 1:N (One User can own many Hotels)
                                    ▼
┌─────────────────────────────────────────────────────────────────────────────────┐
│                                   HOTELS                                        │
├─────────────────────────────────────────────────────────────────────────────────┤
│ PK: _id (ObjectId)                                                              │
│ FK: userId (ObjectId) → USERS._id                                               │
│     name (String)                                                               │
│     city (String)                                                               │
│     country (String)                                                            │
│     description (String)                                                        │
│     type (String)                                                               │
│     adultCount (Number)                                                         │
│     childCount (Number)                                                         │
│     facilities (Array of String)                                                │
│     pricePerNight (Number)                                                      │
│     starRating (Number, 1-5)                                                    │
│     imageUrls (Array of String)                                                 │
│     lastUpdated (Date)                                                          │
│     bookings (Embedded Array)                                                   │
└─────────────────────────────────────────────────────────────────────────────────┘
                                    │
                                    │ 1:N (One Hotel can have many Bookings)
                                    ▼
┌─────────────────────────────────────────────────────────────────────────────────┐
│                                 BOOKINGS                                        │
│                              (Embedded in Hotels)                               │
├─────────────────────────────────────────────────────────────────────────────────┤
│ PK: _id (ObjectId)                                                              │
│ FK: userId (String) → USERS._id                                                 │
│     firstName (String)                                                          │
│     lastName (String)                                                           │
│     email (String)                                                              │
│     adultCount (Number)                                                         │
│     childCount (Number)                                                         │
│     checkIn (Date)                                                              │
│     checkOut (Date)                                                             │
│     totalCost (Number)                                                          │
│     createdAt (Date)                                                            │
└─────────────────────────────────────────────────────────────────────────────────┘
```

## Detailed Entity Descriptions

### 1. USERS Entity

**Primary Key**: `_id` (ObjectId)

**Attributes**:
- `email` (String, Unique, Required)
  - **Description**: User's email address for login
  - **Constraints**: Must be unique, valid email format
  - **Index**: Yes (for fast login lookups)

- `password` (String, Required)
  - **Description**: Hashed password using bcrypt
  - **Constraints**: Minimum 6 characters, hashed with 8 salt rounds
  - **Security**: Never stored in plain text

- `firstName` (String, Required)
  - **Description**: User's first name
  - **Constraints**: Required field

- `lastName` (String, Required)
  - **Description**: User's last name
  - **Constraints**: Required field

- `role` (Enum, Required)
  - **Description**: User's role in the system
  - **Values**: "user", "hotel owner", "admin"
  - **Constraints**: Must be one of the predefined values

- `clickedHotels` (Array of ObjectId)
  - **Description**: Array of hotel IDs that user has clicked/viewed
  - **Purpose**: For potential future recommendation algorithms
  - **Relationship**: References HOTELS._id

**Indexes**:
- `email` (Unique Index)
- `role` (Index for role-based queries)

### 2. HOTELS Entity

**Primary Key**: `_id` (ObjectId)

**Foreign Key**: `userId` (ObjectId) → USERS._id

**Attributes**:
- `userId` (ObjectId, Required)
  - **Description**: Reference to the hotel owner
  - **Relationship**: Many-to-One with USERS
  - **Constraints**: Must reference existing user

- `name` (String, Required)
  - **Description**: Hotel name
  - **Constraints**: Required field

- `city` (String, Required)
  - **Description**: City where hotel is located
  - **Constraints**: Required field
  - **Index**: Yes (for search functionality)

- `country` (String, Required)
  - **Description**: Country where hotel is located
  - **Constraints**: Required field
  - **Index**: Yes (for search functionality)

- `description` (String, Required)
  - **Description**: Detailed hotel description
  - **Constraints**: Required field
  - **Index**: Text index (for search functionality)

- `type` (String, Required)
  - **Description**: Type of hotel (e.g., "All Inclusive", "Boutique")
  - **Constraints**: Required field
  - **Index**: Yes (for filtering)

- `adultCount` (Number, Required)
  - **Description**: Maximum number of adults the hotel can accommodate
  - **Constraints**: Must be positive number

- `childCount` (Number, Required)
  - **Description**: Maximum number of children the hotel can accommodate
  - **Constraints**: Must be positive number

- `facilities` (Array of String, Required)
  - **Description**: List of hotel facilities/amenities
  - **Constraints**: Must be non-empty array
  - **Index**: Yes (for filtering)

- `pricePerNight` (Number, Required)
  - **Description**: Price per night in GBP
  - **Constraints**: Must be positive number
  - **Index**: Yes (for sorting and filtering)

- `starRating` (Number, Required)
  - **Description**: Hotel star rating (1-5 stars)
  - **Constraints**: Must be between 1 and 5
  - **Index**: Yes (for filtering and sorting)

- `imageUrls` (Array of String, Required)
  - **Description**: URLs of hotel images stored in Cloudinary
  - **Constraints**: Must be non-empty array

- `lastUpdated` (Date, Required)
  - **Description**: Timestamp of last hotel update
  - **Constraints**: Automatically set on creation/update
  - **Index**: Yes (for sorting by recent updates)

- `bookings` (Embedded Array)
  - **Description**: Array of booking documents embedded in hotel
  - **Relationship**: One-to-Many with BOOKINGS

**Indexes**:
- `userId` (Index for hotel owner queries)
- `city` (Index for location search)
- `country` (Index for location search)
- `type` (Index for type filtering)
- `pricePerNight` (Index for price sorting/filtering)
- `starRating` (Index for rating filtering)
- `facilities` (Index for facility filtering)
- `lastUpdated` (Index for recent updates)
- Compound indexes for complex queries

### 3. BOOKINGS Entity (Embedded)

**Primary Key**: `_id` (ObjectId)

**Foreign Key**: `userId` (String) → USERS._id

**Attributes**:
- `userId` (String, Required)
  - **Description**: Reference to the user who made the booking
  - **Relationship**: Many-to-One with USERS
  - **Constraints**: Must reference existing user

- `firstName` (String, Required)
  - **Description**: Guest's first name
  - **Constraints**: Required field

- `lastName` (String, Required)
  - **Description**: Guest's last name
  - **Constraints**: Required field

- `email` (String, Required)
  - **Description**: Guest's email address
  - **Constraints**: Required field, valid email format

- `adultCount` (Number, Required)
  - **Description**: Number of adults in booking
  - **Constraints**: Must be positive number

- `childCount` (Number, Required)
  - **Description**: Number of children in booking
  - **Constraints**: Must be non-negative number

- `checkIn` (Date, Required)
  - **Description**: Check-in date
  - **Constraints**: Must be future date

- `checkOut` (Date, Required)
  - **Description**: Check-out date
  - **Constraints**: Must be after check-in date

- `totalCost` (Number, Required)
  - **Description**: Total cost of the booking
  - **Constraints**: Must be positive number

- `createdAt` (Date)
  - **Description**: Timestamp when booking was created
  - **Constraints**: Automatically set on creation

## Relationships

### 1. USERS ↔ HOTELS (1:N)
- **Type**: One-to-Many
- **Description**: One user (hotel owner) can own many hotels
- **Foreign Key**: `HOTELS.userId` references `USERS._id`
- **Constraints**: 
  - Hotel owner must exist in USERS table
  - Hotel owner must have role "hotel owner" or "admin"

### 2. HOTELS ↔ BOOKINGS (1:N)
- **Type**: One-to-Many (Embedded)
- **Description**: One hotel can have many bookings
- **Implementation**: Bookings are embedded as subdocuments in HOTELS
- **Constraints**:
  - Booking dates must not overlap for same hotel
  - Total guests must not exceed hotel capacity

### 3. USERS ↔ BOOKINGS (1:N)
- **Type**: One-to-Many
- **Description**: One user can make many bookings
- **Foreign Key**: `BOOKINGS.userId` references `USERS._id`
- **Constraints**: User must exist in USERS table

### 4. USERS ↔ HOTELS (M:N via clickedHotels)
- **Type**: Many-to-Many
- **Description**: Users can click/view many hotels, hotels can be viewed by many users
- **Implementation**: Array of hotel IDs in USERS.clickedHotels
- **Purpose**: For recommendation algorithms

## Database Indexes

### Primary Indexes
- `USERS._id` (Primary Key)
- `HOTELS._id` (Primary Key)
- `BOOKINGS._id` (Primary Key)

### Unique Indexes
- `USERS.email` (Unique)

### Single Field Indexes
- `USERS.role`
- `HOTELS.userId`
- `HOTELS.city`
- `HOTELS.country`
- `HOTELS.type`
- `HOTELS.pricePerNight`
- `HOTELS.starRating`
- `HOTELS.lastUpdated`
- `BOOKINGS.userId`
- `BOOKINGS.checkIn`
- `BOOKINGS.checkOut`

### Compound Indexes
- `HOTELS.city + HOTELS.pricePerNight + HOTELS.starRating` (For search queries)
- `HOTELS.type + HOTELS.facilities` (For filtering)
- `BOOKINGS.checkIn + BOOKINGS.checkOut` (For availability checking)

### Text Indexes
- `HOTELS.name` (Text index for search)
- `HOTELS.city` (Text index for search)
- `HOTELS.description` (Text index for search)

## Data Integrity Constraints

### Entity Integrity
- All primary keys are ObjectId (MongoDB's default)
- Primary keys are automatically generated and unique

### Referential Integrity
- `HOTELS.userId` must reference existing `USERS._id`
- `BOOKINGS.userId` must reference existing `USERS._id`
- Hotel owners must have appropriate role ("hotel owner" or "admin")

### Domain Integrity
- Email addresses must be valid format
- Passwords must be minimum 6 characters
- Star ratings must be 1-5
- Prices must be positive numbers
- Dates must be valid and logical (checkOut > checkIn)
- Guest counts must be positive numbers

### Business Rules
- Hotel capacity: `adultCount + childCount` represents total capacity
- Booking overlap prevention: No overlapping bookings for same hotel
- Role-based access: Only hotel owners can manage their hotels
- Payment validation: All bookings require successful payment

---

**Document Version**: 1.0.0  
**Last Updated**: December 2024  
**Database**: MongoDB  
**ODM**: Mongoose  
**Architecture**: Document-based with embedded subdocuments 