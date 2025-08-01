# Hotel Booking System - Data Flow Diagram (DFD)

## Level 0 DFD (Context Diagram)

```
┌─────────────────┐    User Data    ┌─────────────────────┐
│                 │ ──────────────► │                     │
│   External      │                 │   Hotel Booking     │
│   Users         │                 │   System            │
│                 │ ◄────────────── │                     │
│                 │   Hotel Data    │                     │
└─────────────────┘                 └─────────────────────┘
                                              │
                                              │ Payment Data
                                              ▼
                                    ┌─────────────────────┐
                                    │   External          │
                                    │   Payment Gateway   │
                                    │   (Stripe)          │
                                    └─────────────────────┘
```

## Level 1 DFD (System Overview)

```
┌─────────────────┐    User Input   ┌─────────────────────┐
│                 │ ──────────────► │                     │
│   External      │                 │   1.0 User          │
│   Users         │                 │   Authentication    │
│                 │ ◄────────────── │                     │
│                 │   Auth Response │                     │
└─────────────────┘                 └─────────────────────┘
                                              │
                                              │ User Data
                                              ▼
                                    ┌─────────────────────┐
                                    │   D1: Users         │
                                    │   Database          │
                                    └─────────────────────┘

┌─────────────────┐    Search       ┌─────────────────────┐
│                 │ ──────────────► │                     │
│   External      │   Criteria      │   2.0 Hotel         │
│   Users         │                 │   Search &          │
│                 │ ◄────────────── │   Filter            │
│                 │   Search        │                     │
└─────────────────┘   Results       └─────────────────────┘
                                              │
                                              │ Hotel Data
                                              ▼
                                    ┌─────────────────────┐
                                    │   D2: Hotels        │
                                    │   Database          │
                                    └─────────────────────┘

┌─────────────────┐    Hotel        ┌─────────────────────┐
│                 │ ──────────────► │                     │
│   External      │   Management    │   3.0 Hotel         │
│   Hotel Owners  │   Data          │   Management        │
│                 │ ◄────────────── │                     │
│                 │   Management    │                     │
└─────────────────┘   Response      └─────────────────────┘

┌─────────────────┐    Booking      ┌─────────────────────┐
│                 │ ──────────────► │                     │
│   External      │   Request       │   4.0 Booking       │
│   Users         │                 │   Processing        │
│                 │ ◄────────────── │                     │
│                 │   Booking       │                     │
└─────────────────┘   Confirmation  └─────────────────────┘
                                              │
                                              │ Payment Data
                                              ▼
                                    ┌─────────────────────┐
                                    │   External          │
                                    │   Payment Gateway   │
                                    │   (Stripe)          │
                                    └─────────────────────┘

┌─────────────────┐    Hotel ID     ┌─────────────────────┐
│                 │ ──────────────► │                     │
│   External      │                 │   5.0 Price-Based   │
│   Users         │                 │   Recommendations   │
│                 │ ◄────────────── │                     │
│                 │   Hotel         │                     │
└─────────────────┘   Suggestions   └─────────────────────┘
```

## Level 2 DFD (Detailed Processes)

### Process 1.0: User Authentication

```
┌─────────────────┐    Login/       ┌─────────────────────┐
│                 │ ──────────────► │                     │
│   External      │   Register      │   1.1 Validate      │
│   Users         │   Data          │   Input Data        │
│                 │ ◄────────────── │                     │
│                 │   Validation    │                     │
└─────────────────┘   Errors        └─────────────────────┘
                                              │
                                              │ Valid Data
                                              ▼
                                    ┌─────────────────────┐
                                    │   1.2 Hash          │
                                    │   Password          │
                                    └─────────────────────┘
                                              │
                                              │ Hashed Data
                                              ▼
                                    ┌─────────────────────┐
                                    │   1.3 Store/        │
                                    │   Verify User       │
                                    └─────────────────────┘
                                              │
                                              │ User Data
                                              ▼
                                    ┌─────────────────────┐
                                    │   D1: Users         │
                                    │   Database          │
                                    └─────────────────────┘
                                              │
                                              │ JWT Token
                                              ▼
                                    ┌─────────────────────┐
                                    │   1.4 Generate      │
                                    │   JWT Token         │
                                    └─────────────────────┘
```

### Process 2.0: Hotel Search & Filter

```
┌─────────────────┐    Search       ┌─────────────────────┐
│                 │ ──────────────► │                     │
│   External      │   Criteria      │   2.1 Construct     │
│   Users         │                 │   Search Query      │
│                 │ ◄────────────── │                     │
│                 │   Search        │                     │
└─────────────────┘   Results       └─────────────────────┘
                                              │
                                              │ Query
                                              ▼
                                    ┌─────────────────────┐
                                    │   2.2 Execute       │
                                    │   Database Query    │
                                    └─────────────────────┘
                                              │
                                              │ Raw Results
                                              ▼
                                    ┌─────────────────────┐
                                    │   D2: Hotels        │
                                    │   Database          │
                                    └─────────────────────┘
                                              │
                                              │ Filtered Data
                                              ▼
                                    ┌─────────────────────┐
                                    │   2.3 Apply         │
                                    │   Sorting &         │
                                    │   Pagination        │
                                    └─────────────────────┘
```

### Process 3.0: Hotel Management

```
┌─────────────────┐    Hotel        ┌─────────────────────┐
│                 │ ──────────────► │                     │
│   External      │   Data          │   3.1 Validate      │
│   Hotel Owners  │                 │   Hotel Data        │
│                 │ ◄────────────── │                     │
│                 │   Validation    │                     │
└─────────────────┘   Response      └─────────────────────┘
                                              │
                                              │ Valid Data
                                              ▼
                                    ┌─────────────────────┐
                                    │   3.2 Upload        │
                                    │   Images to         │
                                    │   Cloudinary        │
                                    └─────────────────────┘
                                              │
                                              │ Image URLs
                                              ▼
                                    ┌─────────────────────┐
                                    │   3.3 Store         │
                                    │   Hotel Data        │
                                    └─────────────────────┘
                                              │
                                              │ Hotel Data
                                              ▼
                                    ┌─────────────────────┐
                                    │   D2: Hotels        │
                                    │   Database          │
                                    └─────────────────────┘
```

### Process 4.0: Booking Processing

```
┌─────────────────┐    Booking      ┌─────────────────────┐
│                 │ ──────────────► │                     │
│   External      │   Request       │   4.1 Check         │
│   Users         │                 │   Availability      │
│                 │ ◄────────────── │                     │
│                 │   Availability  │                     │
└─────────────────┘   Status        └─────────────────────┘
                                              │
                                              │ Available
                                              ▼
                                    ┌─────────────────────┐
                                    │   4.2 Create        │
                                    │   Payment Intent    │
                                    └─────────────────────┘
                                              │
                                              │ Payment Intent
                                              ▼
                                    ┌─────────────────────┐
                                    │   External          │
                                    │   Payment Gateway   │
                                    │   (Stripe)          │
                                    └─────────────────────┘
                                              │
                                              │ Payment Success
                                              ▼
                                    ┌─────────────────────┐
                                    │   4.3 Create        │
                                    │   Booking           │
                                    └─────────────────────┘
                                              │
                                              │ Booking Data
                                              ▼
                                    ┌─────────────────────┐
                                    │   D2: Hotels        │
                                    │   Database          │
                                    └─────────────────────┘
```

### Process 5.0: Price-Based Recommendations

```
┌─────────────────┐    Hotel ID     ┌─────────────────────┐
│                 │ ──────────────► │                     │
│   External      │                 │   5.1 Fetch         │
│   Users         │                 │   Current Hotel     │
│                 │ ◄────────────── │                     │
│                 │   Hotel         │                     │
└─────────────────┘   Suggestions   └─────────────────────┘
                                              │
                                              │ Current Price
                                              ▼
                                    ┌─────────────────────┐
                                    │   5.2 Calculate     │
                                    │   Price Similarity  │
                                    └─────────────────────┘
                                              │
                                              │ Scored Hotels
                                              ▼
                                    ┌─────────────────────┐
                                    │   D2: Hotels        │
                                    │   Database          │
                                    └─────────────────────┘
                                              │
                                              │ Similarity Scores
                                              ▼
                                    ┌─────────────────────┐
                                    │   5.3 Sort &        │
                                    │   Select Top 5      │
                                    └─────────────────────┘
```

## Data Stores

### D1: Users Database
- **Content**: User profiles, authentication data, roles
- **Operations**: Create, Read, Update, Delete
- **Key Fields**: _id, email, password, firstName, lastName, role, clickedHotels

### D2: Hotels Database
- **Content**: Hotel information, bookings, availability
- **Operations**: Create, Read, Update, Delete
- **Key Fields**: _id, userId, name, city, country, description, type, adultCount, childCount, facilities, pricePerNight, starRating, imageUrls, lastUpdated, bookings

## External Entities

### External Users
- **Regular Users**: Browse hotels, make bookings, view recommendations
- **Hotel Owners**: Manage their hotel properties
- **Admins**: System administration and oversight

### External Payment Gateway (Stripe)
- **Purpose**: Process secure payments
- **Data Flow**: Payment intents, card confirmations, transaction status

### External Image Storage (Cloudinary)
- **Purpose**: Store and serve hotel images
- **Data Flow**: Image uploads, URL generation, image serving

## Data Flow Summary

| **Process** | **Input** | **Output** | **Data Store** |
|-------------|-----------|------------|----------------|
| User Authentication | Login/Register Data | JWT Token | Users DB |
| Hotel Search | Search Criteria | Filtered Results | Hotels DB |
| Hotel Management | Hotel Data | Hotel Records | Hotels DB |
| Booking Processing | Booking Request | Booking Confirmation | Hotels DB |
| Recommendations | Hotel ID | Similar Hotels | Hotels DB |

---

**Document Version**: 1.0.0  
**Last Updated**: December 2024  
**System**: Hotel Booking System  
**Architecture**: MERN Stack (MongoDB, Express, React, Node.js) 