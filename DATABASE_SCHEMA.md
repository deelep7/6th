# 🗄️ Database Schema Diagram

## 📊 Collections Overview

```
┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐
│      User       │    │     Hotel       │    │    Booking      │
├─────────────────┤    ├─────────────────┤    ├─────────────────┤
│ _id: ObjectId   │    │ _id: ObjectId   │    │ _id: ObjectId   │
│ email: String   │    │ userId: String  │    │ userId: String  │
│ password: String│    │ name: String    │    │ hotelId: String │
│ firstName: String│   │ city: String    │    │ firstName: String│
│ lastName: String│    │ country: String │    │ lastName: String│
│ role: Enum      │    │ description: Str│    │ email: String   │
│ clickedHotels[] │    │ type: String    │    │ adultCount: Num │
└─────────────────┘    │ adultCount: Num │    │ childCount: Num │
                       │ childCount: Num │    │ checkIn: Date   │
                       │ facilities[]    │    │ checkOut: Date  │
                       │ pricePerNight: N│    │ totalCost: Num  │
                       │ starRating: Num │    │ createdAt: Date │
                       │ imageUrls[]     │    └─────────────────┘
                       │ lastUpdated: Date│
                       └─────────────────┘
```

## 🔗 Relationships

```
User (1) ──── (Many) Hotel
User (1) ──── (Many) Booking
Hotel (1) ─── (Many) Booking
User (1) ──── (Many) clickedHotels [Hotel References]
```

## 📋 Detailed Schema

### 🧑‍💼 **User Collection**
```javascript
{
  _id: ObjectId,                    // Primary Key
  email: String,                    // Unique, Required
  password: String,                 // Hashed, Required
  firstName: String,                // Required
  lastName: String,                 // Required
  role: "user" | "hotel owner" | "admin",  // Required
  clickedHotels: [ObjectId]         // References to Hotel._id (max 20)
}
```

**Indexes:**
- `email` (Unique)
- `clickedHotels` (Array)

**Hooks:**
- Pre-save: Password hashing with bcrypt

---

### 🏨 **Hotel Collection**
```javascript
{
  _id: ObjectId,                    // Primary Key
  userId: String,                   // References User._id
  name: String,                     // Required
  city: String,                     // Required
  country: String,                  // Required
  description: String,              // Required
  type: String,                     // Required
  adultCount: Number,               // Required
  childCount: Number,               // Required
  facilities: [String],             // Array of facility names
  pricePerNight: Number,            // Required
  starRating: Number,               // Required (1-5)
  imageUrls: [String],              // Array of image URLs
  lastUpdated: Date,                // Required
  availability: {                   // Computed field (not stored)
    isFullyBooked: Boolean,
    availableCapacity: Number,
    totalBookedGuests: Number,
    maxCapacity: Number,
    overlappingBookings: Number,
    totalBookedAdults: Number,
    totalBookedChildren: Number,
    availableAdults: Number,
    availableChildren: Number,
    isAdultsFullyBooked: Boolean,
    isChildrenFullyBooked: Boolean,
    maxAdults: Number,
    maxChildren: Number
  },
  algorithm: {                      // Computed field (not stored)
    avgClickedPrice: Number,
    priceDiff: Number,
    priceSimilarity: String,
    rank: String,
    isClicked: Boolean
  }
}
```

**Indexes:**
- `userId` (for hotel owner queries)
- `city` (for search)
- `country` (for search)
- `type` (for filtering)
- `pricePerNight` (for sorting)
- `starRating` (for filtering)

---

### 📅 **Booking Collection**
```javascript
{
  _id: ObjectId,                    // Primary Key
  userId: String,                   // References User._id
  hotelId: String,                  // References Hotel._id
  firstName: String,                // Required
  lastName: String,                 // Required
  email: String,                    // Required
  adultCount: Number,               // Required
  childCount: Number,               // Required
  checkIn: Date,                    // Required
  checkOut: Date,                   // Required
  totalCost: Number,                // Required
  createdAt: Date                   // Auto-generated
}
```

**Indexes:**
- `userId` (for user's bookings)
- `hotelId` (for hotel's bookings)
- `checkIn` (for date queries)
- `checkOut` (for date queries)
- `{hotelId, checkIn, checkOut}` (Compound index for availability)

---

## 🔍 **Query Patterns & Performance**

### **User Operations:**
```javascript
// Find user by email (login)
User.findOne({ email: "user@example.com" })

// Get user's bookings
Booking.find({ userId: "user123" })

// Get user's clicked hotels
User.findById("user123").populate("clickedHotels")
```

### **Hotel Operations:**
```javascript
// Search hotels
Hotel.find({
  $or: [
    { city: /london/i },
    { country: /uk/i },
    { name: /luxury/i }
  ],
  pricePerNight: { $lte: 200 },
  adultCount: { $gte: 2 }
})

// Get hotel with availability
Hotel.findById("hotel123")
// + Booking.find({ hotelId: "hotel123", checkIn: { $lt: checkOut }, checkOut: { $gt: checkIn } })

// Get hotel owner's properties
Hotel.find({ userId: "owner123" })
```

### **Booking Operations:**
```javascript
// Check availability (compound index)
Booking.find({
  hotelId: "hotel123",
  $or: [
    {
      checkIn: { $lt: checkOutDate },
      checkOut: { $gt: checkInDate }
    }
  ]
})

// Get hotel's bookings
Booking.find({ hotelId: "hotel123" })

// Get user's bookings with hotel details
Booking.find({ userId: "user123" })
// + Hotel.find({ _id: { $in: hotelIds } })
```

---

## 🎯 **Data Flow Examples**

### **1. Hotel Search & Booking Flow:**
```
1. User searches hotels → Hotel.find() with filters
2. User clicks hotel → POST /hotels/:id/click → User.clickedHotels.push()
3. User books hotel → POST /hotels/:id/bookings → Booking.create()
4. Check availability → Booking.find() with date overlap
```

### **2. Recommendation Flow:**
```
1. User visits hotel → Record click in clickedHotels
2. Get recommendations → Analyze clickedHotels patterns
3. Return similar hotels → Price-based algorithm
```

### **3. User Dashboard Flow:**
```
1. Get user's bookings → Booking.find({ userId })
2. Get associated hotels → Hotel.find({ _id: { $in: hotelIds } })
3. Display combined data → Merge booking + hotel info
```

---

## 📈 **Scalability Considerations**

### **Current Optimizations:**
- ✅ Compound indexes for complex queries
- ✅ Array size limits (clickedHotels: max 20)
- ✅ Efficient date range queries
- ✅ Proper field indexing

### **Future Optimizations:**
- 🔄 Pagination for large result sets
- 🔄 Caching for frequently accessed data
- 🔄 Read replicas for heavy read workloads
- 🔄 Sharding for horizontal scaling

---

## 🔒 **Security & Validation**

### **Data Validation:**
- ✅ Required fields enforced
- ✅ Email uniqueness
- ✅ Password hashing
- ✅ Role-based access control
- ✅ Date validation for bookings

### **Access Control:**
- ✅ JWT token verification
- ✅ Role-based middleware
- ✅ User ownership validation
- ✅ Hotel owner permissions

---

## 📊 **Analytics & Insights**

### **Available Metrics:**
- User engagement (clickedHotels length)
- Popular hotels (click frequency)
- Booking patterns (dates, durations)
- Revenue tracking (totalCost aggregation)
- Hotel performance (occupancy rates)

### **Query Examples:**
```javascript
// Most clicked hotels
db.users.aggregate([
  { $unwind: "$clickedHotels" },
  { $group: { _id: "$clickedHotels", count: { $sum: 1 } } },
  { $sort: { count: -1 } }
])

// Revenue by hotel
db.bookings.aggregate([
  { $group: { _id: "$hotelId", totalRevenue: { $sum: "$totalCost" } } },
  { $sort: { totalRevenue: -1 } }
])

// User engagement levels
db.users.aggregate([
  { $project: { 
    engagementLevel: { 
      $cond: [
        { $gt: [{ $size: "$clickedHotels" }, 10] }, "High",
        { $cond: [{ $gt: [{ $size: "$clickedHotels" }, 5] }, "Medium", "Low"] }
      ]
    }
  }}
])
``` 