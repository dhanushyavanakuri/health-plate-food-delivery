# HealthPlate

**A health-aware restaurant and dish discovery application.**

🌐 **Live Website:** (https://wtpbl-rho.vercel.app/)

HealthPlate helps users discover restaurants and dishes based on health conditions, dietary preferences, and allergies. Unlike conventional food delivery platforms that primarily filter by cuisine or price, HealthPlate focuses on dish-level health information.

## Features

- **JWT authentication:** User accounts with saved health profiles, dietary preferences, and allergies.
- **Restaurant discovery:** Browse and filter restaurants by cuisine, price, rating, and distance.
- **Health-aware dish tagging:** Add dishes with health tags and allergen information.
- **Safe for Me:** Filter dishes according to a saved health profile.
- **Health condition filters:** Manually select multiple health-related filters.
- **Community voting:** Upvote or downvote dish-tag accuracy, with confidence-based ranking.
- **Dish details:** View health tags, allergens, voting information, and ordering tips.

## Tech Stack

| Layer | Technology |
|---|---|
| Frontend | React 18, Vite, Tailwind CSS |
| Backend | Node.js, Express |
| Database | MongoDB with Mongoose |
| External service | Google Places API (optional) |
| Deployment | Vercel (frontend), Render (backend) |

## Quick Start

You need two terminals to run the backend and frontend separately.

### 1. Backend

```bash
cd backend
npm install
npm run seed
npm run dev
```

The backend runs at `http://localhost:5000`.

### 2. Frontend

Open a second terminal:

```bash
cd frontend
npm install
npm run dev
```

The frontend runs at `http://localhost:5173`.

### Demo Login

- **Email:** `demo@healthplate.app`
- **Password:** `demo1234`

Use these credentials only if the demo account is available in your running environment.

## Database Configuration

For local development, the application can use an in-memory MongoDB fallback when `MONGODB_URI` is empty.

The in-memory database is temporary: its data is lost when the server process exits. Use the project's seed-and-serve command, if configured, to seed and run the backend in the same process.

For persistent data, configure a MongoDB Atlas connection using `MONGODB_URI`.

## Environment Variables

Create your backend environment file from the provided example:

```bash
# Run from the project root
copy backend\.env.example backend\.env
```

| Variable | Purpose |
|---|---|
| `PORT` | Backend API port; defaults to `5000` |
| `MONGODB_URI` | MongoDB connection string |
| `JWT_SECRET` | Secret used to sign authentication tokens |
| `JWT_EXPIRES_IN` | Token lifetime |
| `CLIENT_URL` | Allowed frontend origin or origins |
| `GOOGLE_PLACES_API_KEY` | Optional Google Places API key |
| `DEFAULT_LAT` / `DEFAULT_LNG` | Fallback map coordinates |

Set a strong, unique `JWT_SECRET` in production. Never commit real secrets or `.env` files to GitHub.

## Google Places Integration

Google Places integration is optional. Without an API key, the application can use its seeded restaurant data.

When configured, the nearby-search endpoint can retrieve restaurant results from Google Places and store them in the database. Health tags should be supplied by the community rather than assumed from external restaurant data.

## Location Handling

The frontend requests browser geolocation and can fall back to a configured demo location when permission is unavailable. The interface should indicate which location is being used.

## API Reference

### Authentication

| Method | Endpoint | Purpose |
|---|---|---|
| POST | `/api/auth/signup` | Register a user |
| POST | `/api/auth/login` | Authenticate a user |
| GET | `/api/auth/me` | Retrieve the current user's profile |
| PATCH | `/api/auth/me` | Update profile preferences and allergies |
| GET | `/api/auth/config` | Retrieve health-tag and allergen options |

### Restaurants

| Method | Endpoint | Purpose |
|---|---|---|
| GET | `/api/restaurants` | List and filter restaurants |
| GET | `/api/restaurants/:id` | Retrieve restaurant details |
| GET | `/api/restaurants/meta/filters` | Retrieve available filters |
| POST | `/api/restaurants/search-nearby` | Search for nearby restaurants |

Restaurant listing filters may include health tags, excluded tags, allergens, cuisine, price, search terms, radius, sorting, pagination, and the `safe_for_me` option.

### Dishes

| Method | Endpoint | Purpose |
|---|---|---|
| GET | `/api/dishes` | List and filter dishes |
| GET | `/api/dishes/:id` | Retrieve dish details |
| POST | `/api/dishes` | Add a dish |
| PATCH | `/api/dishes/:id` | Update dish information |
| POST | `/api/dishes/:id/vote` | Vote on dish-tag accuracy |

Voting behavior includes one vote per user, with the ability to retract or change a vote, as implemented by the backend.

## Testing

Run the backend tests from the backend directory:

```bash
cd backend
npm run smoke
npm run verify
```

These commands are the project's configured API checks. Their exact coverage depends on the current implementation.

## Data Model Notes

### Vote Confidence

The documented confidence formula is:

`(up - down) / (total + 5)`

The added constant reduces the influence of dishes with very few votes.

### Seed Data

The project seed data includes untagged dishes to demonstrate how health-related filters exclude dishes that do not meet the selected criteria.

## Deployment

### Backend — Render

1. Create a Render Web Service connected to the repository.
2. Set the root directory to `backend`.
3. Set the build command to `npm install`.
4. Set the start command to `npm start`.
5. Configure `JWT_SECRET`, `MONGODB_URI`, and `CLIENT_URL`.
6. Seed the persistent database as required by the project.

### Frontend — Vercel

1. Import the repository into Vercel.
2. Set the root directory to `frontend`.
3. Select the Vite framework preset.
4. Configure `VITE_API_URL` to point to the deployed backend URL, without a trailing slash.
5. Deploy and test the website.

Check the project's deployment configuration and environment variables before redeploying.

## Future Enhancements

The following features are not currently implemented according to the project notes:

- AI-based health-tag suggestions from ingredient lists
- Real-time restaurant menu synchronization
- Online payments and food ordering
