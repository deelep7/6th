# Simple Hotel & Recommendation Modules

## Overview
Small, focused modules for hotel creation and recommendations that can be easily included in your project report.

## 📁 Module Structure

```
backend/src/modules/
├── simple-hotel.service.ts          # Basic hotel operations
└── simple-recommendation.service.ts # Simple recommendations

frontend/src/modules/
├── SimpleHotelForm.tsx              # Hotel creation form
└── SimpleRecommendations.tsx        # Recommendations display
```

## 🏨 Hotel Module

### Backend: SimpleHotelService

**Location:** `backend/src/modules/simple-hotel.service.ts`

**Features:**
- Get all hotels
- Get hotel by ID
- Search hotels by name, city, country, description
- Create new hotel
- Update hotel
- Delete hotel

**Key Methods:**
```typescript
// Get all hotels
static async getAllHotels(): Promise<HotelType[]>

// Search hotels
static async searchHotels(query: string): Promise<HotelType[]>

// Create hotel
static async createHotel(hotelData: any): Promise<HotelType>
```

### Frontend: SimpleHotelForm

**Location:** `frontend/src/modules/SimpleHotelForm.tsx`

**Features:**
- Clean, simple form for hotel creation
- Basic validation
- Facility selection with checkboxes
- Responsive design
- Loading states

**Form Fields:**
- Hotel Name
- Type (Budget, Business, Luxury, Resort)
- City & Country
- Description
- Adult & Child Capacity
- Price per Night
- Star Rating
- Facilities (checkboxes)
- Image URL

## 🎯 Recommendation Module

### Backend: SimpleRecommendationService

**Location:** `backend/src/modules/simple-recommendation.service.ts`

**Features:**
- Price-based recommendations
- Popular hotels (high star rating)
- Fallback to random hotels

**Algorithm:**
```typescript
// Price similarity calculation
const priceDiff = Math.abs(hotel.pricePerNight - currentHotel.pricePerNight);
const similarity = Math.max(0, 100 - (priceDiff / currentHotel.pricePerNight * 100));
```

**Key Methods:**
```typescript
// Get recommendations based on current hotel
static async getRecommendations(hotelId?: string): Promise<HotelType[]>

// Get popular hotels
static async getPopularHotels(): Promise<HotelType[]>
```

### Frontend: SimpleRecommendations

**Location:** `frontend/src/modules/SimpleRecommendations.tsx`

**Features:**
- Display recommendations in grid layout
- Loading and error states
- Similarity score badges
- Responsive design

## 🚀 Usage

### Backend Integration
```typescript
import { SimpleHotelService } from './modules/simple-hotel.service';
import { SimpleRecommendationService } from './modules/simple-recommendation.service';

// Use in your routes
app.get('/api/hotels', async (req, res) => {
  const hotels = await SimpleHotelService.getAllHotels();
  res.json(hotels);
});

app.get('/api/recommendations', async (req, res) => {
  const recommendations = await SimpleRecommendationService.getRecommendations(req.query.hotelId);
  res.json(recommendations);
});
```

### Frontend Integration
```tsx
import { SimpleHotelForm } from './modules/SimpleHotelForm';
import { SimpleRecommendations } from './modules/SimpleRecommendations';

// In your routes
<Route path="/add-hotel" element={<SimpleHotelForm />} />
<Route path="/recommendations" element={<SimpleRecommendations />} />
```

## 📊 Key Features

### Hotel Creation
- ✅ Simple form with essential fields
- ✅ Real-time validation
- ✅ Facility selection
- ✅ Image URL input
- ✅ Responsive design

### Recommendations
- ✅ Price-based similarity algorithm
- ✅ Visual similarity scores
- ✅ Loading states
- ✅ Error handling
- ✅ Grid layout display

## 🔧 Technical Details

### Dependencies
- **Backend:** Express, Mongoose, TypeScript
- **Frontend:** React, React Query, Tailwind CSS

### API Endpoints
- `GET /api/hotels` - Get all hotels
- `GET /api/hotels/:id` - Get hotel by ID
- `POST /api/hotels` - Create hotel
- `GET /api/recommendations?hotelId=123` - Get recommendations

### Data Flow
1. User fills hotel form
2. Form data sent to backend
3. Hotel saved to database
4. Recommendations calculated based on price similarity
5. Results displayed in grid layout

## 📝 Project Report Integration

These modules demonstrate:
- **Clean Architecture:** Separation of concerns
- **TypeScript:** Type safety throughout
- **React Patterns:** Hooks, state management
- **API Design:** RESTful endpoints
- **User Experience:** Loading states, error handling
- **Algorithm Implementation:** Price-based recommendations

Perfect for showcasing your technical skills in a concise, focused manner. 