# Hotel Booking System - Project Report

## Overview
A full-stack hotel booking application built with React, Node.js, Express, MongoDB, and TypeScript. The system features user authentication, hotel management, booking functionality, and an intelligent price-based recommendation system.

## Tech Stack

### Frontend
- **React 18** with TypeScript
- **React Query** for data fetching and caching
- **React Router** for navigation
- **Tailwind CSS** for styling
- **Stripe** for payment processing
- **React Hook Form** for form management

### Backend
- **Node.js** with Express
- **TypeScript** for type safety
- **MongoDB** with Mongoose ODM
- **JWT** for authentication
- **Stripe** API for payments
- **Cookie-parser** for session management

## Key Features

### 1. User Authentication & Authorization
- User registration and login
- JWT-based authentication with cookies
- Role-based access control (User, Admin)
- Protected routes and middleware

### 2. Hotel Management
- Hotel owners can add, edit, and manage their properties
- Admin can view and delete any hotel
- Hotel details include: name, description, images, facilities, pricing, capacity
- Star ratings and hotel types

### 3. Search & Filtering
- Advanced search with multiple filters:
  - Destination (city, country, name, description)
  - Date range (check-in/check-out)
  - Guest count (adults and children)
  - Price range
  - Star ratings
  - Hotel types
  - Facilities
- Sorting options (price, star rating)
- Pagination support

### 4. Booking System
- Real-time availability checking
- Separate adult and children capacity tracking
- Booking validation and conflict detection
- Stripe payment integration
- Booking history for users

### 5. Recommendation System
- **Price-Based Content Filtering Algorithm**
- Shows hotels similar in price to the currently viewed hotel
- Real-time recommendations based on current hotel price
- Fallback to random hotels when no context available

## Recommendation Algorithm

### Algorithm Type: Price-Based Content Filtering

#### How It Works
1. **Input**: Current hotel ID (from query parameters)
2. **Target**: Find hotels with similar pricing to the current hotel
3. **Scoring**: Calculate price similarity percentage
4. **Output**: Top 5 hotels sorted by price similarity

#### Formula
```typescript
const priceDiff = Math.abs(hotel.pricePerNight - currentPrice);
const priceSimilarity = Math.max(0, 100 - (priceDiff / currentPrice * 100));
```

#### Example
- **Current Hotel**: ₨100
- **Hotel A**: ₨90 → 90% similar (diff: ₨10)
- **Hotel B**: ₨110 → 90% similar (diff: ₨10)
- **Hotel C**: ₨80 → 80% similar (diff: ₨20)

#### Features
- ✅ **Real-time**: Based on current hotel, not historical data
- ✅ **Price-focused**: Simple, effective price-based matching
- ✅ **Robust**: Handles edge cases with fallbacks
- ✅ **Fast**: Simple calculations, no complex ML
- ✅ **Clean UI**: No algorithm details displayed to users

## API Endpoints

### Authentication
- `POST /api/auth/register` - User registration
- `POST /api/auth/login` - User login
- `POST /api/auth/logout` - User logout
- `GET /api/users/me` - Get current user

### Hotels
- `GET /api/hotels` - Get all hotels
- `GET /api/hotels/search` - Search hotels with filters
- `GET /api/hotels/:id` - Get hotel by ID with availability
- `GET /api/hotels/recommendations` - Get recommendations (with optional hotelId)
- `POST /api/hotels/:hotelId/click` - Record hotel click
- `POST /api/hotels` - Add new hotel (owner only)
- `PUT /api/hotels/:hotelId` - Update hotel (owner only)
- `DELETE /api/hotels/:hotelId` - Delete hotel (owner only)

### Bookings
- `POST /api/hotels/:hotelId/bookings/payment-intent` - Create payment intent
- `POST /api/hotels/:hotelId/bookings` - Create booking
- `GET /api/my-bookings` - Get user's bookings

### Admin
- `GET /api/hotels/admin/all` - Get all hotels with owner details
- `DELETE /api/hotels/admin/:hotelId` - Delete any hotel

## Database Schema

### User Model
```typescript
{
  _id: ObjectId,
  email: string,
  password: string,
  firstName: string,
  lastName: string,
  role: "user" | "admin",
  clickedHotels: string[], // Array of hotel IDs for recommendations
  createdAt: Date,
  updatedAt: Date
}
```

### Hotel Model
```typescript
{
  _id: ObjectId,
  userId: ObjectId, // Owner ID
  name: string,
  city: string,
  country: string,
  description: string,
  type: string,
  adultCount: number,
  childCount: number,
  facilities: string[],
  pricePerNight: number,
  starRating: number,
  imageUrls: string[],
  lastUpdated: Date,
  bookings: Booking[]
}
```

### Booking Model
```typescript
{
  _id: ObjectId,
  userId: ObjectId,
  firstName: string,
  lastName: string,
  email: string,
  adultCount: number,
  childCount: number,
  checkIn: Date,
  checkOut: Date,
  totalCost: number,
  createdAt: Date
}
```

## Key Components

### Frontend Components
- **Layout**: Main layout with navigation
- **SearchResultsCard**: Hotel display card
- **GuestInfoForm**: Booking form
- **BookingForm**: Payment form with Stripe
- **Pagination**: Search results pagination
- **Filters**: Search and filter components

### Backend Middleware
- **verifyToken**: JWT authentication
- **requireRole**: Role-based authorization
- **Validation**: Request validation with express-validator

## Error Handling

### Frontend
- React Query error boundaries
- User-friendly error messages
- Loading states for all async operations
- Form validation with React Hook Form

### Backend
- Try-catch blocks for all async operations
- Proper HTTP status codes
- Detailed error messages for debugging
- Input validation with express-validator

## Security Features

1. **Authentication**: JWT tokens stored in HTTP-only cookies
2. **Authorization**: Role-based access control
3. **Input Validation**: Server-side validation for all inputs
4. **Password Hashing**: bcrypt for password security
5. **CORS**: Configured for frontend domain
6. **Rate Limiting**: Basic rate limiting on auth endpoints

## Performance Optimizations

1. **React Query**: Caching and background updates
2. **Pagination**: Limit data transfer
3. **Image Optimization**: Responsive images
4. **Database Indexing**: Indexed on frequently queried fields
5. **Lazy Loading**: Components loaded on demand

## Recent Updates & Improvements

### Recommendation System Evolution
1. **Initial**: Basic collaborative filtering based on user click history
2. **Content-Based**: Multi-factor scoring (type, city, facilities, price, stars)
3. **Hybrid**: Combination of content-based and user behavior
4. **Final**: Simplified price-based content filtering

### UI/UX Improvements
- Removed algorithm similarity score badges
- Removed "Great Value!" promotional tags
- Simplified price display to show only price and location
- Clean, focused hotel cards
- Prominent price display on detail pages

### Code Quality
- Removed all console.log statements
- Fixed TypeScript errors
- Improved error handling
- Better type safety throughout

## Deployment

### Environment Variables
```env
MONGODB_URL=mongodb://localhost:27017/hotel-booking
JWT_SECRET=your-jwt-secret
STRIPE_API_KEY=your-stripe-secret-key
FRONTEND_URL=http://localhost:5173
```

### Build Commands
```bash
# Backend
npm run build
npm start

# Frontend
npm run build
npm run preview
```

## Future Enhancements

1. **Advanced Recommendations**: Machine learning-based recommendations
2. **Real-time Features**: WebSocket for live availability updates
3. **Mobile App**: React Native version
4. **Analytics**: User behavior tracking and insights
5. **Multi-language**: Internationalization support
6. **Reviews & Ratings**: User review system
7. **Email Notifications**: Booking confirmations and reminders

## Conclusion

This hotel booking system provides a complete solution for hotel management and booking with a focus on user experience and intelligent recommendations. The price-based recommendation algorithm ensures users see relevant options while maintaining simplicity and performance.

The system is production-ready with proper security, error handling, and scalability considerations. The modular architecture allows for easy maintenance and future enhancements. 