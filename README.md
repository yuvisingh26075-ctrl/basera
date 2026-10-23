Basera

A hotel booking website for stays across India. It has a Spring Boot (Java) backend and a single-page HTML frontend, served together by one app.

Features
Search stays by city or name, with check-in, check-out and guests
Filter by type (Hotel, Villa, Homestay, Resort), maximum price and rating, and sort the results
Stay pages with a photo gallery, description and amenities
Prices calculated on the server: nights, rooms, 12% GST, 5% service fee and total in ₹
Room availability checked for every night of the stay, with protection against double booking
Sign up and sign in, with passwords hashed using bcrypt
Book a stay, view it under My trips, and cancel it
Wishlist saved to your account
Responsive layout with light and dark themes
Tech stack
Part	Technology
Backend	Java 17, Spring Boot 3
Database	SQLite, through Spring JDBC
Auth	JWT (7-day tokens), bcrypt password hashing
Frontend	One HTML file with plain JavaScript, no build step
Getting started

You need Java 17 or newer and Maven.

bash
git clone https://github.com/YOUR-USERNAME/basera.git
cd basera
JWT_SECRET="replace-with-a-random-string-of-32-or-more-characters" mvn spring-boot:run

On Windows PowerShell:

powershell
$env:JWT_SECRET="replace-with-a-random-string-of-32-or-more-characters"
mvn spring-boot:run

Open http://localhost:3000. The app creates the database and adds 12 sample stays on first run.

Settings
Environment variable	Purpose	Default
JWT_SECRET	Key used to sign login tokens. Required, 32+ characters.	none
PORT	Port the server listens on	3000
DB_FILE	Path of the SQLite database file	basera.db

Never commit your JWT_SECRET. Set it as an environment variable on your computer or server.

API

All routes are under /api. Routes marked "Sign in" need an Authorization: Bearer <token> header.

Method and path	Purpose	Sign in
POST /auth/signup	Create an account	No
POST /auth/login	Sign in and receive a token	No
GET /me	Current user	Yes
GET /hotels	List stays. Filters: q, type, maxPrice, minRating, sort (pa, pd, rt)	No
GET /hotels/{id}	One stay	No
GET /hotels/{id}/quote	Price and availability. Needs checkIn, checkOut, guests	No
POST /bookings	Book a stay. Body: hotelId, checkIn, checkOut, guests	Yes
GET /bookings	Your bookings	Yes
DELETE /bookings/{ref}	Cancel a booking	Yes
GET /wishlist	Saved stay ids	Yes
PUT /wishlist/{id}	Save a stay	Yes
DELETE /wishlist/{id}	Remove a saved stay	Yes

Errors come back as {"error": "message"} with a matching HTTP status.

Project structure
pom.xml
src/main/java/com/basera/
  BaseraApplication.java   app entry point and security headers
  ApiController.java       all API routes
  AuthService.java         tokens and login rate limiting
  Db.java                  tables and sample data
  ApiException.java        error handling
src/main/resources/
  application.properties
  static/index.html        the frontend
Known limitations
Stay photos are drawn illustrations. Replace the scene values and the img() calls in index.html with real photo URLs.
No online payment yet. Guests pay at the property.
The login token is stored in the browser's localStorage. For production, switch to an httpOnly cookie.
Booking protection works for a single server. Running several servers needs PostgreSQL and database-level locking.
SQLite suits development and small launches. Move to PostgreSQL for heavier traffic.
Roadmap
Real photos stored on S3 or Cloudinary
Payments with Razorpay
Booking confirmation emails
Host accounts to list and manage properties
Reviews from guests who completed a stay
License

Released under the MIT License. See the LICENSE file for details.
