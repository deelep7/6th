# Unit Testing Documentation - Hotel Booking System

## 4.2.1 Test Case for Unit Testing

Different unit testing is performed on the individual module of this Hotel Booking System.

## i) User Authentication

### Table 4.1: Unit Test for User Login

| ID | Description | Steps | Expected Result |
|----|-------------|-------|-----------------|
| **1.1** | Valid Login Credentials | 1. Open the login page<br>2. Enter valid email and password<br>3. Click on the "Sign In" button | User is successfully logged in and redirected to the dashboard |
| **1.2** | Invalid Login Credentials | 1. Open the login page<br>2. Enter invalid email and password<br>3. Click on the "Sign In" button | An error message is displayed indicating invalid login credentials |
| **1.3** | Empty Login Fields | 1. Open the login page<br>2. Leave email and password fields empty<br>3. Click on the "Sign In" button | Validation error messages are displayed for required fields |
| **1.4** | Invalid Email Format | 1. Open the login page<br>2. Enter invalid email format and password<br>3. Click on the "Sign In" button | Error message displayed for invalid email format |
| **1.5** | User Logout | 1. User is logged in<br>2. Click on "Sign Out" button<br>3. Confirm logout | User is successfully logged out and redirected to home page |

### Table 4.2: Unit Test for User Registration

| ID | Description | Steps | Expected Result |
|----|-------------|-------|-----------------|
| **2.1** | Valid User Registration | 1. Navigate to the registration page<br>2. Enter valid First Name, Last Name, Email, Password, and Role<br>3. Click on "Create Account" | User is successfully registered and can login |
| **2.2** | Duplicate Email Registration | 1. Navigate to the registration page<br>2. Enter existing email with other valid details<br>3. Click on "Create Account" | Error message displayed for duplicate email |
| **2.3** | Weak Password Registration | 1. Navigate to the registration page<br>2. Enter valid details with weak password<br>3. Click on "Create Account" | Error message displayed for password strength requirements |
| **2.4** | Invalid Email Format Registration | 1. Navigate to the registration page<br>2. Enter invalid email format with other valid details<br>3. Click on "Create Account" | Error message displayed for invalid email format |
| **2.5** | Empty Required Fields | 1. Navigate to the registration page<br>2. Leave required fields empty<br>3. Click on "Create Account" | Validation error messages displayed for required fields |

## ii) Hotel Management

### Table 4.3: Unit Test for Hotel Creation

| ID | Description | Steps | Expected Result |
|----|-------------|-------|-----------------|
| **3.1** | Add New Hotel | 1. Navigate to the Hotel Management section<br>2. Click "Add Hotel" button<br>3. Enter Name, City, Country, Description, Type, Guest Capacity, Facilities, Price, Star Rating, and Images<br>4. Click on "Save Hotel" | Hotel is successfully added to the system |
| **3.2** | Add Hotel with Invalid Star Rating | 1. Navigate to the Hotel Management section<br>2. Click "Add Hotel" button<br>3. Enter valid details with star rating > 5<br>4. Click on "Save Hotel" | Error message displayed for invalid star rating (must be 1-5) |
| **3.3** | Add Hotel with Missing Required Fields | 1. Navigate to the Hotel Management section<br>2. Click "Add Hotel" button<br>3. Leave required fields empty<br>4. Click on "Save Hotel" | Validation error messages displayed for required fields |
| **3.4** | Add Hotel with Invalid Price | 1. Navigate to the Hotel Management section<br>2. Click "Add Hotel" button<br>3. Enter negative price value<br>4. Click on "Save Hotel" | Error message displayed for invalid price |
| **3.5** | Add Hotel with Invalid Guest Capacity | 1. Navigate to the Hotel Management section<br>2. Click "Add Hotel" button<br>3. Enter zero or negative guest capacity<br>4. Click on "Save Hotel" | Error message displayed for invalid guest capacity |

### Table 4.4: Unit Test for Hotel Management

| ID | Description | Steps | Expected Result |
|----|-------------|-------|-----------------|
| **4.1** | View Hotel Details | 1. Navigate to the Hotel Management section<br>2. Click on a hotel from the list<br>3. View hotel information | Hotel details are displayed correctly |
| **4.2** | Update Hotel Information | 1. Navigate to the Hotel Management section<br>2. Click on "Edit" icon for a hotel<br>3. Modify hotel details (name, price, facilities)<br>4. Click on "Update Hotel" | Hotel information is updated successfully |
| **4.3** | Delete Hotel | 1. Navigate to the Hotel Management section<br>2. Click on "Delete" icon for a hotel<br>3. Confirm deletion<br>4. Click on "Delete" | Hotel is deleted successfully from the system |
| **4.4** | Search Hotels | 1. Navigate to the Hotel Management section<br>2. Enter search criteria (name, city, type)<br>3. Click on "Search" | Relevant hotels are displayed based on search criteria |
| **4.5** | Filter Hotels by Type | 1. Navigate to the Hotel Management section<br>2. Select hotel type filter (Hotel, Resort, Motel)<br>3. Apply filter | Hotels are filtered by selected type |

## iii) Booking Management

### Table 4.5: Unit Test for Booking Creation

| ID | Description | Steps | Expected Result |
|----|-------------|-------|-----------------|
| **5.1** | Create Valid Booking | 1. Navigate to hotel detail page<br>2. Click "Book Now" button<br>3. Enter guest information (First Name, Last Name, Email)<br>4. Select check-in and check-out dates<br>5. Enter guest count (adults and children)<br>6. Click "Confirm Booking" | Booking is created successfully and confirmation is displayed |
| **5.2** | Create Booking with Invalid Dates | 1. Navigate to hotel detail page<br>2. Click "Book Now" button<br>3. Select check-out date before check-in date<br>4. Click "Confirm Booking" | Error message displayed for invalid date selection |
| **5.3** | Create Booking with Past Dates | 1. Navigate to hotel detail page<br>2. Click "Book Now" button<br>3. Select past check-in date<br>4. Click "Confirm Booking" | Error message displayed for past date selection |
| **5.4** | Create Booking Exceeding Capacity | 1. Navigate to hotel detail page<br>2. Click "Book Now" button<br>3. Enter guest count exceeding hotel capacity<br>4. Click "Confirm Booking" | Error message displayed for exceeding guest capacity |
| **5.5** | Create Booking with Invalid Email | 1. Navigate to hotel detail page<br>2. Click "Book Now" button<br>3. Enter invalid email format<br>4. Click "Confirm Booking" | Error message displayed for invalid email format |

### Table 4.6: Unit Test for Booking Management

| ID | Description | Steps | Expected Result |
|----|-------------|-------|-----------------|
| **6.1** | View My Bookings | 1. Navigate to "My Bookings" section<br>2. View list of user's bookings | All user bookings are displayed with details |
| **6.2** | View Booking Details | 1. Navigate to "My Bookings" section<br>2. Click on a specific booking<br>3. View booking information | Detailed booking information is displayed |
| **6.3** | Cancel Booking | 1. Navigate to "My Bookings" section<br>2. Click on "Cancel" button for a booking<br>3. Confirm cancellation<br>4. Click "Cancel Booking" | Booking is cancelled successfully |
| **6.4** | Search Bookings | 1. Navigate to "My Bookings" section<br>2. Enter search criteria (hotel name, dates)<br>3. Click "Search" | Relevant bookings are displayed based on search criteria |
| **6.5** | Filter Bookings by Status | 1. Navigate to "My Bookings" section<br>2. Select status filter (Active, Cancelled, Completed)<br>3. Apply filter | Bookings are filtered by selected status |

## iv) Search and Filtering

### Table 4.7: Unit Test for Hotel Search

| ID | Description | Steps | Expected Result |
|----|-------------|-------|-----------------|
| **7.1** | Search by Destination | 1. Navigate to search page<br>2. Enter destination (city/country)<br>3. Click "Search" | Hotels in specified destination are displayed |
| **7.2** | Search by Date Range | 1. Navigate to search page<br>2. Select check-in and check-out dates<br>3. Click "Search" | Available hotels for selected dates are displayed |
| **7.3** | Search by Guest Count | 1. Navigate to search page<br>2. Enter number of adults and children<br>3. Click "Search" | Hotels with sufficient capacity are displayed |
| **7.4** | Search with No Results | 1. Navigate to search page<br>2. Enter criteria with no matching hotels<br>3. Click "Search" | "No hotels found" message is displayed |
| **7.5** | Advanced Search | 1. Navigate to search page<br>2. Enter multiple criteria (destination, dates, guests)<br>3. Click "Search" | Hotels matching all criteria are displayed |

### Table 4.8: Unit Test for Price Filter

| ID | Description | Steps | Expected Result |
|----|-------------|-------|-----------------|
| **8.1** | Filter by Maximum Price | 1. Navigate to search results<br>2. Select maximum price from dropdown<br>3. Apply filter | Hotels within price range are displayed |
| **8.2** | Filter by Price Range | 1. Navigate to search results<br>2. Select different price ranges<br>3. Apply filter | Hotels within selected price ranges are displayed |
| **8.3** | Clear Price Filter | 1. Navigate to search results with price filter applied<br>2. Click "Clear Filters"<br>3. Verify results | All hotels are displayed without price restriction |
| **8.4** | Filter with No Results | 1. Navigate to search results<br>2. Select very low price range<br>3. Apply filter | "No hotels found" message is displayed |
| **8.5** | Multiple Price Filters | 1. Navigate to search results<br>2. Apply multiple price filters sequentially<br>3. Verify results | Results are updated correctly for each filter |

### Table 4.9: Unit Test for Star Rating Filter

| ID | Description | Steps | Expected Result |
|----|-------------|-------|-----------------|
| **9.1** | Filter by Star Rating | 1. Navigate to search results<br>2. Select star rating (1-5 stars)<br>3. Apply filter | Hotels with selected star rating are displayed |
| **9.2** | Filter by Multiple Star Ratings | 1. Navigate to search results<br>2. Select multiple star ratings<br>3. Apply filter | Hotels with any of selected star ratings are displayed |
| **9.3** | Filter by Minimum Star Rating | 1. Navigate to search results<br>2. Select minimum star rating<br>3. Apply filter | Hotels with star rating >= selected are displayed |
| **9.4** | Clear Star Rating Filter | 1. Navigate to search results with star filter applied<br>2. Click "Clear Filters"<br>3. Verify results | All hotels are displayed without star rating restriction |
| **9.5** | Star Rating with No Results | 1. Navigate to search results<br>2. Select 5-star rating only<br>3. Apply filter | Only 5-star hotels are displayed or "No hotels found" |

### Table 4.10: Unit Test for Hotel Type Filter

| ID | Description | Steps | Expected Result |
|----|-------------|-------|-----------------|
| **10.1** | Filter by Hotel Type | 1. Navigate to search results<br>2. Select hotel type (Hotel, Resort, Motel)<br>3. Apply filter | Hotels of selected type are displayed |
| **10.2** | Filter by Multiple Types | 1. Navigate to search results<br>2. Select multiple hotel types<br>3. Apply filter | Hotels of any selected type are displayed |
| **10.3** | Clear Type Filter | 1. Navigate to search results with type filter applied<br>2. Click "Clear Filters"<br>3. Verify results | All hotels are displayed without type restriction |
| **10.4** | Type Filter with No Results | 1. Navigate to search results<br>2. Select hotel type not available<br>3. Apply filter | "No hotels found" message is displayed |
| **10.5** | Type Filter Combinations | 1. Navigate to search results<br>2. Apply type filter with other filters<br>3. Verify results | Combined filter results are displayed correctly |

## v) Recommendation System

### Table 4.11: Unit Test for Recommendation Engine

| ID | Description | Steps | Expected Result |
|----|-------------|-------|-----------------|
| **11.1** | Auto Recommendation Strategy | 1. Navigate to recommendations page<br>2. Select "Auto" strategy<br>3. View recommendations | System automatically selects best recommendation strategy |
| **11.2** | Price-Based Recommendations | 1. Navigate to recommendations page<br>2. Select "Price-based" strategy<br>3. View recommendations | Hotels similar in price to current hotel are displayed |
| **11.3** | User Behavior Recommendations | 1. Navigate to recommendations page<br>2. Select "User Behavior" strategy<br>3. View recommendations | Hotels based on user's browsing history are displayed |
| **11.4** | Popularity Recommendations | 1. Navigate to recommendations page<br>2. Select "Popularity" strategy<br>3. View recommendations | Highly-rated and popular hotels are displayed |
| **11.5** | Random Recommendations | 1. Navigate to recommendations page<br>2. Select "Random" strategy<br>3. View recommendations | Random selection of hotels for discovery are displayed |

### Table 4.12: Unit Test for Recommendation Statistics

| ID | Description | Steps | Expected Result |
|----|-------------|-------|-----------------|
| **12.1** | View Recommendation Stats | 1. Navigate to recommendations page<br>2. Click "Show Statistics" toggle<br>3. View statistics | Recommendation statistics are displayed |
| **12.2** | View Strategy Information | 1. Navigate to recommendations page<br>2. Click "Show Statistics"<br>3. View strategy details | Current strategy and description are displayed |
| **12.3** | View Click History Count | 1. Navigate to recommendations page<br>2. Click "Show Statistics"<br>3. View click history | Number of hotels clicked by user is displayed |
| **12.4** | View Recommendation Scores | 1. Navigate to recommendations page<br>2. Click "Show Statistics"<br>3. View recommendation scores | Individual hotel recommendation scores are displayed |
| **12.5** | View Recommendation Reasons | 1. Navigate to recommendations page<br>2. Click "Show Statistics"<br>3. View recommendation reasons | Reasons for each recommendation are displayed |

## vi) Admin Management

### Table 4.13: Unit Test for Admin Functions

| ID | Description | Steps | Expected Result |
|----|-------------|-------|-----------------|
| **13.1** | View All Users | 1. Navigate to Admin panel<br>2. Click "User Management"<br>3. View user list | All system users are displayed with details |
| **13.2** | View All Hotels | 1. Navigate to Admin panel<br>2. Click "Hotel Management"<br>3. View hotel list | All system hotels are displayed with details |
| **13.3** | View All Bookings | 1. Navigate to Admin panel<br>2. Click "Booking Management"<br>3. View booking list | All system bookings are displayed with details |
| **13.4** | Update User Role | 1. Navigate to Admin panel<br>2. Select user from list<br>3. Change user role<br>4. Save changes | User role is updated successfully |
| **13.5** | Delete User | 1. Navigate to Admin panel<br>2. Select user from list<br>3. Click "Delete User"<br>4. Confirm deletion | User is deleted from system |

## vii) Error Handling and Validation

### Table 4.14: Unit Test for Error Handling

| ID | Description | Steps | Expected Result |
|----|-------------|-------|-----------------|
| **14.1** | Network Error Handling | 1. Simulate network disconnection<br>2. Perform any API operation<br>3. Check error display | Appropriate error message is displayed |
| **14.2** | Server Error Handling | 1. Simulate server error (500)<br>2. Perform API operation<br>3. Check error display | Server error message is displayed |
| **14.3** | Validation Error Display | 1. Submit form with invalid data<br>2. Check error messages<br>3. Verify field highlighting | Validation errors are displayed correctly |
| **14.4** | Session Expiry Handling | 1. Simulate session expiry<br>2. Perform authenticated operation<br>3. Check redirect behavior | User is redirected to login page |
| **14.5** | Permission Denied Handling | 1. Access restricted resource<br>2. Check error message<br>3. Verify access control | Permission denied message is displayed |

## viii) Performance Testing

### Table 4.15: Unit Test for Performance

| ID | Description | Steps | Expected Result |
|----|-------------|-------|-----------------|
| **15.1** | Page Load Performance | 1. Navigate to main pages<br>2. Measure load time<br>3. Verify performance | Pages load within acceptable time (<3 seconds) |
| **15.2** | Search Response Time | 1. Perform hotel search<br>2. Measure response time<br>3. Verify results | Search results appear within acceptable time (<2 seconds) |
| **15.3** | Image Loading Performance | 1. Navigate to hotel detail page<br>2. Check image loading time<br>3. Verify display | Images load within acceptable time (<1 second) |
| **15.4** | Form Submission Performance | 1. Submit booking form<br>2. Measure processing time<br>3. Verify response | Form submission completes within acceptable time (<2 seconds) |
| **15.5** | Filter Application Performance | 1. Apply multiple filters<br>2. Measure response time<br>3. Verify results | Filtered results appear within acceptable time (<1 second) |

## Test Execution Summary

### Test Environment Requirements
- **Browser**: Chrome, Firefox, Safari, Edge (latest versions)
- **Screen Resolution**: 1920x1080, 1366x768, 375x667 (mobile)
- **Network**: High-speed internet connection
- **Database**: Test database with sample data

### Test Data Requirements
- Sample users with different roles (Customer, Hotel Owner, Admin)
- Sample hotels with various types, prices, and ratings
- Sample bookings with different statuses
- Sample search criteria and filter combinations

### Test Execution Checklist
- [ ] All test cases executed
- [ ] Expected results verified
- [ ] Error scenarios tested
- [ ] Performance benchmarks met
- [ ] Cross-browser compatibility verified
- [ ] Mobile responsiveness tested
- [ ] Accessibility requirements met

### Test Results Documentation
- Test execution date and time
- Test environment details
- Pass/Fail status for each test case
- Screenshots of failed test cases
- Performance metrics recorded
- Bug reports for failed tests
- Recommendations for improvements 