# Hotel Booking System - Hotel & Recommendation Modules

This document provides comprehensive documentation for the Hotel and Recommendation modules created for the hotel booking system.

## 📁 Module Structure

```
backend/src/modules/
├── common/
│   └── errors.ts                    # Custom error classes
├── hotel/
│   ├── hotel.service.ts            # Hotel business logic
│   ├── hotel.controller.ts         # HTTP request handlers
│   └── hotel.routes.ts             # Hotel API routes
└── recommendation/
    ├── recommendation.service.ts   # Recommendation algorithms
    ├── recommendation.controller.ts # HTTP request handlers
    └── recommendation.routes.ts    # Recommendation API routes

frontend/src/modules/
├── hotel/
│   └── HotelManagement.tsx         # Hotel management UI
├── recommendation/
│   └── RecommendationEngine.tsx    # Recommendation UI
└── index.ts                        # Module exports
```

## 🏨 Hotel Module

### Backend Components

#### HotelService (`backend/src/modules/hotel/hotel.service.ts`)

**Features:**
- Complete CRUD operations for hotels
- Advanced search with multiple filters
- Real-time availability checking
- Comprehensive data validation
- Capacity management (adults/children)

**Key Methods:**
```typescript
// Create a new hotel
static async createHotel(hotelData: HotelCreateData): Promise<HotelType>

// Get hotel by ID with availability check
static async getHotelById(hotelId: string, checkIn?: string, checkOut?: string)

// Search hotels with filters
static async searchHotels(params: HotelSearchParams): Promise<HotelSearchResponse>

// Update hotel
static async updateHotel(hotelId: string, userId: string, updateData: HotelUpdateData)

// Delete hotel
static async deleteHotel(hotelId: string, userId: string): Promise<void>

// Check availability
static async checkAvailability(hotelId: string, checkIn: Date, checkOut: Date)
```

#### HotelController (`backend/src/modules/hotel/hotel.controller.ts`)

**Features:**
- HTTP request handling
- Error management with custom error types
- Input validation
- Role-based access control
- Admin functionality

**API Endpoints:**
- `GET /api/hotels/search` - Search hotels with filters
- `GET /api/hotels/:id` - Get hotel by ID
- `POST /api/hotels` - Create new hotel
- `PUT /api/hotels/:hotelId` - Update hotel
- `DELETE /api/hotels/:hotelId` - Delete hotel
- `GET /api/hotels/my-hotels` - Get user's hotels
- `GET /api/hotels/admin/all` - Admin: Get all hotels
- `DELETE /api/hotels/admin/:hotelId` - Admin: Delete any hotel

### Frontend Components

#### HotelManagement (`frontend/src/modules/hotel/HotelManagement.tsx`)

**Features:**
- Modern, responsive UI with Tailwind CSS
- Form validation with real-time feedback
- Image URL management
- Facility selection with checkboxes
- Capacity and pricing configuration
- Loading states and error handling

**Props:**
```typescript
interface HotelManagementProps {
  mode: 'view' | 'create' | 'edit';
  hotelId?: string;
}
```

**Usage:**
```tsx
// Create new hotel
<HotelManagement mode="create" />

// Edit existing hotel
<HotelManagement mode="edit" hotelId="hotel123" />
```

## 🎯 Recommendation Module

### Backend Components

#### RecommendationService (`backend/src/modules/recommendation/recommendation.service.ts`)

**Features:**
- Multiple recommendation strategies
- Price-based content filtering
- User behavior analysis
- Popularity-based recommendations
- Random discovery fallback
- Click tracking for user behavior

**Recommendation Strategies:**

1. **Price-Based Content Filtering**
   - Recommends hotels similar in price to current hotel
   - Formula: `similarity = 100 - (priceDiff / currentPrice * 100)`
   - Best for: Contextual recommendations

2. **User Behavior Analysis**
   - Based on user's click history
   - Considers hotel type, city, and price range
   - Best for: Personalized recommendations

3. **Popularity Strategy**
   - High star ratings and recent updates
   - Best for: General recommendations

4. **Random Strategy**
   - Fallback for discovery
   - Best for: Exploration

**Key Methods:**
```typescript
// Get recommendations
static async getRecommendations(params: RecommendationParams): Promise<ScoredHotel[]>

// Get recommendations with strategy info
static async getRecommendationsWithStrategy(params: RecommendationParams)

// Record hotel click
static async recordHotelClick(userId: string, hotelId: string): Promise<void>

// Get user click history
static async getUserClickHistory(userId: string): Promise<string[]>
```

#### RecommendationController (`backend/src/modules/recommendation/recommendation.controller.ts`)

**Features:**
- Strategy-based recommendation endpoints
- Click tracking and analytics
- User behavior insights
- Performance monitoring

**API Endpoints:**
- `GET /api/recommendations` - Get recommendations
- `GET /api/recommendations/with-strategy` - Get recommendations with strategy info
- `GET /api/recommendations/stats` - Get recommendation statistics
- `POST /api/recommendations/:hotelId/click` - Record hotel click
- `GET /api/recommendations/click-history` - Get user click history

### Frontend Components

#### RecommendationEngine (`frontend/src/modules/recommendation/RecommendationEngine.tsx`)

**Features:**
- Interactive strategy selector
- Real-time analytics dashboard
- Visual recommendation scoring
- User engagement tracking
- Responsive grid layout
- Loading states and error handling

**Key Features:**
- Strategy selection (Auto, Price-based, User behavior, Popularity, Random)
- Analytics panel with detailed metrics
- Click tracking for improved recommendations
- Visual similarity scores
- User activity dashboard

**Usage:**
```tsx
// Show recommendations for current hotel
<RecommendationEngine />

// Show recommendations for specific hotel
<RecommendationEngine hotelId="hotel123" />
```

## 🔧 Common Components

### Error Handling (`backend/src/modules/common/errors.ts`)

**Custom Error Classes:**
- `ValidationError` - Input validation errors
- `AuthenticationError` - Authentication failures
- `AuthorizationError` - Permission denied
- `NotFoundError` - Resource not found
- `DatabaseError` - Database operation failures

## 🚀 Usage Examples

### Backend Integration

```typescript
// In your main app file
import hotelRoutes from './modules/hotel/hotel.routes';
import recommendationRoutes from './modules/recommendation/recommendation.routes';

app.use('/api/hotels', hotelRoutes);
app.use('/api/recommendations', recommendationRoutes);
```

### Frontend Integration

```tsx
// In your App.tsx or router
import { HotelManagement, RecommendationEngine } from './modules';

// Routes
<Route path="/add-hotel" element={<HotelManagement mode="create" />} />
<Route path="/edit-hotel/:id" element={<HotelManagement mode="edit" />} />
<Route path="/recommendations" element={<RecommendationEngine />} />
```

## 📊 API Documentation

### Hotel Search Parameters

```typescript
interface HotelSearchParams {
  destination?: string;      // Search in name, city, country, description
  checkIn?: string;          // ISO date string
  checkOut?: string;         // ISO date string
  adultCount?: string;       // Minimum adult capacity
  childCount?: string;       // Minimum child capacity
  page?: string;            // Page number for pagination
  stars?: string[];         // Star ratings filter
  types?: string[];         // Hotel types filter
  facilities?: string[];    // Facilities filter
  maxPrice?: string;        // Maximum price per night
  sortOption?: string;      // Sort by: starRating, pricePerNightAsc, pricePerNightDesc
}
```

### Recommendation Parameters

```typescript
interface RecommendationParams {
  hotelId?: string;         // Current hotel ID for context
  userId?: string;          // User ID for personalized recommendations
  limit?: number;           // Number of recommendations (default: 5)
}
```

## 🎨 UI Features

### Hotel Management
- **Form Validation**: Real-time validation with visual feedback
- **Image Management**: Dynamic image URL addition/removal
- **Facility Selection**: Checkbox-based facility selection
- **Capacity Configuration**: Separate adult/child capacity tracking
- **Responsive Design**: Mobile-friendly interface

### Recommendation Engine
- **Strategy Visualization**: Icons and descriptions for each strategy
- **Analytics Dashboard**: Detailed metrics and scoring
- **Interactive Elements**: Click tracking and strategy switching
- **Visual Scoring**: Similarity scores displayed on hotel cards
- **User Engagement**: Activity tracking and insights

## 🔒 Security Features

- **Authentication**: JWT-based authentication required for protected routes
- **Authorization**: Role-based access control (User, Admin)
- **Input Validation**: Server-side validation for all inputs
- **Error Handling**: Secure error messages without exposing internals
- **Rate Limiting**: Protection against abuse

## 📈 Performance Optimizations

- **Database Indexing**: Optimized queries with proper indexes
- **Caching**: React Query for frontend caching
- **Pagination**: Efficient data loading
- **Lazy Loading**: Components loaded on demand
- **Error Boundaries**: Graceful error handling

## 🧪 Testing Considerations

### Backend Testing
- Unit tests for service methods
- Integration tests for API endpoints
- Error handling tests
- Authentication/authorization tests

### Frontend Testing
- Component rendering tests
- Form validation tests
- API integration tests
- User interaction tests

## 🔄 Future Enhancements

### Hotel Module
- Image upload functionality
- Advanced analytics dashboard
- Bulk operations for admins
- Hotel comparison features
- Review and rating system

### Recommendation Module
- Machine learning integration
- A/B testing framework
- Advanced user segmentation
- Real-time recommendation updates
- Cross-platform recommendations

## 📝 Configuration

### Environment Variables
```env
# Database
MONGODB_URL=mongodb://localhost:27017/hotel-booking

# Authentication
JWT_SECRET=your-jwt-secret

# API Configuration
API_BASE_URL=http://localhost:3000/api
FRONTEND_URL=http://localhost:5173
```

### Dependencies
```json
{
  "backend": {
    "express": "^4.18.0",
    "mongoose": "^7.0.0",
    "express-validator": "^7.0.0",
    "jsonwebtoken": "^9.0.0"
  },
  "frontend": {
    "react": "^18.0.0",
    "react-query": "^3.39.0",
    "react-router-dom": "^6.0.0",
    "tailwindcss": "^3.0.0"
  }
}
```

## 🤝 Contributing

1. Follow the existing code structure and patterns
2. Add proper TypeScript types for all new features
3. Include error handling for all async operations
4. Write tests for new functionality
5. Update documentation for any API changes

## 📄 License

This module is part of the Hotel Booking System project and follows the same license terms. 