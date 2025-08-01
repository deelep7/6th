# Booking Migration: Embedded to Separate Collection

## Overview
This document describes the migration from embedded bookings (stored within hotel documents) to a separate booking collection in MongoDB.

## Changes Made

### 1. Database Schema Changes

#### New Booking Model (`backend/src/models/booking.ts`)
- Created a separate `Booking` model with its own collection
- Added indexes for better query performance:
  - `userId` index for user bookings
  - `hotelId` index for hotel bookings
  - `checkIn` and `checkOut` indexes for date queries
  - Compound index on `hotelId`, `checkIn`, `checkOut` for availability checks

#### Updated Hotel Model (`backend/src/models/hotel.ts`)
- Removed embedded `bookingSchema`
- Removed `bookings` array from hotel schema
- Hotel documents are now smaller and more focused

#### Updated Types (`backend/src/shared/types.ts`)
- Added `hotelId` and `createdAt` fields to `BookingType`
- Removed `bookings` array from `HotelType`

### 2. API Changes

#### Hotels Route (`backend/src/routes/hotels.ts`)
- **Availability Calculation**: Now queries the separate booking collection for overlapping bookings
- **Booking Creation**: Creates new documents in the booking collection instead of pushing to hotel array
- **Performance**: Better performance for availability checks with proper indexing

#### My Bookings Route (`backend/src/routes/my-bookings.ts`)
- **Fetch User Bookings**: Queries booking collection by `userId`, then fetches associated hotels
- **Cancel Booking**: Directly deletes from booking collection
- **Data Structure**: Returns same structure as before for frontend compatibility

#### My Hotels Route (`backend/src/routes/my-hotels.ts`)
- **View Hotel Bookings**: Queries booking collection by `hotelId`
- **Cancel User Booking**: Deletes from booking collection with proper authorization

### 3. Migration Script

#### Migration File (`backend/src/migrate-bookings.ts`)
- Migrates existing embedded bookings to the new collection
- Preserves all booking data including timestamps
- Removes embedded bookings from hotel documents after successful migration
- Includes verification and logging

## Benefits of the New Structure

### 1. **Better Performance**
- Smaller hotel documents (no embedded arrays)
- Efficient queries with proper indexing
- Faster availability calculations

### 2. **Scalability**
- No document size limits for bookings
- Better for hotels with many bookings
- Easier to implement pagination

### 3. **Data Integrity**
- Proper referential integrity with `hotelId` references
- Easier to maintain data consistency
- Better for analytics and reporting

### 4. **Flexibility**
- Easier to add booking-specific features
- Better support for complex booking queries
- Simplified backup and restore processes

## Migration Process

### Prerequisites
- Backup your database before running migration
- Ensure no active bookings are being created during migration

### Running the Migration

1. **Stop the application** to prevent new bookings during migration

2. **Run the migration script**:
   ```bash
   cd backend
   npm run migrate-bookings
   ```

3. **Verify the migration**:
   - Check migration logs for success messages
   - Verify booking count in new collection
   - Test application functionality

4. **Restart the application** with new code

### Rollback Plan
If migration fails, you can:
1. Restore from backup
2. Keep using the old embedded structure
3. Fix issues and re-run migration

## Testing

After migration, test the following functionality:
- [ ] User can view their bookings
- [ ] User can cancel their bookings
- [ ] Hotel owners can view bookings for their hotels
- [ ] Hotel owners can cancel user bookings
- [ ] Availability calculations work correctly
- [ ] New bookings can be created
- [ ] Search and filtering still work

## Future Enhancements

With the separate booking collection, you can now easily add:
- Booking analytics and reporting
- Advanced booking queries
- Booking history tracking
- Integration with external booking systems
- Automated booking reminders
- Revenue analytics per hotel

## Notes

- The frontend code remains unchanged as the API responses maintain the same structure
- All existing functionality is preserved
- The migration is backward compatible for the API layer
- Performance improvements will be noticeable with larger datasets 