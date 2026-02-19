# HouseHunt: Finding Your Perfect Rental Home

A MERN stack application for renting properties.

## Prerequisites

Before setting up the application, ensure you have the following installed:

### Node.js and npm
- Node.js: JavaScript runtime for server-side development.
- Download: [https://nodejs.org/en/download/](https://nodejs.org/en/download/)
- Installation: [https://nodejs.org/en/download/package-manager/](https://nodejs.org/en/download/package-manager/)

### Express.js
- Web framework for Node.js.
- Install: `npm install express`

### MongoDB
- NoSQL database for data storage.
- Download: [https://www.mongodb.com/try/download/community](https://www.mongodb.com/try/download/community)
- Installation: [https://docs.mongodb.com/manual/installation/](https://docs.mongodb.com/manual/installation/)

### Moment.js
- Library for date/time manipulation.
- Install: `npm install moment`
- Docs: [https://momentjs.com/](https://momentjs.com/)

### React.js
- Library for building user interfaces.
- Install via Create React App: [https://reactjs.org/docs/create-a-new-react-app.html](https://reactjs.org/docs/create-a-new-react-app.html)

### Antd
- React UI library for components.
- Install: `npm install antd`
- Docs: [https://ant.design/docs/react/introduce](https://ant.design/docs/react/introduce)

### HTML, CSS, JavaScript
- Basic knowledge required for development.

### Database Connectivity
- Use Mongoose for MongoDB connection: [https://www.section.io/engineering-education/nodejs-mongoosejs-mongodb/](https://www.section.io/engineering-education/nodejs-mongoosejs-mongodb/)

## Features

- User registration and authentication (Renters, Owners, Admins)
- Property listings with search and filters
- Booking requests and management
- Admin approval for owners
- Real-time messaging (placeholder)

## Tech Stack

- Frontend: React, Antd, Axios
- Backend: Express.js, MongoDB, JWT Authentication
- Database: MongoDB

## Setup

1. Clone the repository
2. Install dependencies for both client and server
3. Set up MongoDB
4. Run the application

### Backend

```bash
cd server
npm install
npm run dev
```

### Backend setup (detailed)

- Copy environment example and set secure values:

```bash
cd server
copy .env.example .env
# then edit .env and set MONGO_URI and JWT_SECRET
```

- Start the server (development with hot reload):

```bash
npm run dev
```

- The server exposes REST endpoints under `/api/*` (auth, properties, bookings, admin).


### Frontend

```bash
cd frontend
npm install
npm start
```

### Frontend setup (detailed)

- Create the frontend folder and initialize a React app (if not present):

```bash
cd frontend
npx create-react-app .
```

- Install the UI libraries used in this project:

```bash
npm install react bootstrap @mui/material @emotion/react @emotion/styled axios moment antd mdb-react-ui-kit react-bootstrap
```

- Start the frontend dev server:

```bash
npm start
```

Notes: The frontend directory in this repository is `frontend/` (not `client/`).

## Environment Variables

Create a .env file in server directory:

```
MONGO_URI=mongodb://localhost:27017/househunt
JWT_SECRET=your_secret_key
```

## Usage

- Register as a renter or owner
- Browse properties
- Send booking requests
- Owners can manage properties
- Admins approve owners

**API Reference**

- **Health:** `GET /api/health` — public: returns service status `{ status, timestamp }`.

- **Auth**:
	- **POST /api/auth/register** — Register a new user.
		- Body: `{ name, email, password, type }` where `type` is `renter` or `owner`.
		- Owners are created with `isApproved: false` and require admin approval.
		- Response: `201` on success.
	- **POST /api/auth/login** — Login and receive a JWT.
		- Body: `{ email, password }`
		- Response: `{ token, user }` (use token in `Authorization: Bearer <token>`).

- **Properties** (`/api/properties`):
	- **GET /** — Public list. Optional query params: `location`, `type`, `minRent`, `maxRent`.
	- **GET /:id** — Public single property details.
	- **POST /** — Create property (requires `Authorization` header).
		- Requires owner account with `isApproved: true`.
		- Body example: `{ propType, address, amount, images, additionalInfo }`.
	- **PUT /:id** — Update property (owner only).
	- **DELETE /:id** — Delete property (owner only).

- **Bookings** (`/api/bookings`) — Authenticated endpoints:
	- **GET /** — Returns bookings where requester is renter or owner.
	- **POST /** — Create booking: body `{ propertyId, message }` creates `status: 'pending'`.
	- **PUT /:id** — Owner updates booking status. Body: `{ status: 'approved'|'rejected' }`.

- **Admin** (`/api/admin`) — Admin-only endpoints:
	- **GET /pending-owners** — List owner applicants awaiting approval.
	- **PUT /approve-owner/:id** — Approve an owner account.

**Authentication**
- Protected endpoints require the header: `Authorization: Bearer <token>`.

**Quick examples**

- Login to get a token:

```bash
curl -X POST http://localhost:5000/api/auth/login \
	-H "Content-Type: application/json" \
	-d '{"email":"alice@example.com","password":"password"}'
```

- Create a booking (authenticated):

```bash
curl -X POST http://localhost:5000/api/bookings \
	-H "Content-Type: application/json" \
	-H "Authorization: Bearer $TOKEN" \
	-d '{"propertyId":"<PROPERTY_ID>","message":"I\'m interested"}'
```

For implementation details see the route handlers in `server/routes/` and schemas in `server/models/`.