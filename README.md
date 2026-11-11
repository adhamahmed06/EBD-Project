# Liberty & Lotus (L&L)

**Course:** Electronic Business Development (BINF 503) · Winter 2026
**Instructor:** Dr. Nourhan Hamdi
**Teaching Assistants:** Mr. Nour Gaser, Mr. Omar Alaa, Ms. Mariam Khalid, Mr. Younes Zied, Mr. Mahmoud Hani

## 1. Team

| Name  | Student ID | Tutorial group | GitHub username |
|---|---|---|---|
| [Hady Ayman] | [16001639] | [T09] | [hadyayman288] |
| [Yehia Elaidy] | [16004902] | [T09] | [yehiaelaidy] |
| [Adham Youssef] | [16002868] | [T9] | [adhamahmed06] |
| [Name] | [ID] | [T#] | [username] |
| [Name] | [ID] | [T#] | [username] |
| [Name] | [ID] | [T#] | [username] |

## 2. The idea

Liberty & Lotus (L&L) is a made-up travel agency website for travelers from anywhere in America who want to see Egypt. Visitors browse ready-made 10-day trips (the pyramids, a Nile cruise, Alexandria), choose a comfort tier, pick the American region they fly from, and book a spot. It solves a simple problem: planning a trip abroad is scattered across many sites, and L&L puts browsing, booking, changing a booking and reviewing a trip in one place. It is a university project with a light, fairytale tone, not a real business.

## 3. Features

### 3.1 The full product

- Trips to more countries than Egypt, plus beach and desert extensions
- Live flight search from every major American airport
- Real payments, invoices and refunds
- Group bookings with a shared trip page for friends and family
- Email and phone notifications when a booking changes
- Photo uploads on reviews
- A map for each trip itinerary
- Multiple languages
- An admin dashboard with booking statistics

### 3.2 The use cases we will build

| # | Use case | What the user does | What they see | What is stored |
|---|---|---|---|---|
| 1 | Registration and login | Registers with name, email and password; logs in with email and password | Their home page, with their name in the header | A User, with the password hashed and a role (traveler or admin) |
| 2 | Manage trips (admin) | An admin creates a trip, edits its details, or deletes it | A trip form, then the updated trips list right after saving | A Trip: title, summary, itinerary days, tiers offered, departure regions |
| 3 | Book a trip | Opens a trip, picks a tier, number of travelers, departure region and preferred month, then confirms | A booking confirmation with the chosen details and a "pending" status | A Booking linking the user and the trip, with tier, travelers, region, month and status |
| 4 | Change or cancel a booking | Opens "My bookings", edits the tier, travelers or month, or cancels the booking | The updated booking in the list, or the booking marked cancelled | The Booking is updated, or its status is set to cancelled |
| 5 | Write a review | Writes, edits or deletes a review on a trip they booked; an admin can delete any review | The trip page with its list of reviews, updated after each change | A Review: user, trip, rating (1 to 5), comment |
| 6 | Save trips to a wishlist | Taps a heart on a trip to save it, or removes it from the wishlist page | A wishlist page listing the saved trips, updated after each tap | The list of saved Trip ids on the User |

## 4. Who owns what

| Member | Use case |
|---|---|
| [Adham Ahmed] | 1. Registration and login |
| [yehia elaidy] | 2. Manage trips (admin) |
| [Name] | 3. Book a trip |
| [Name] | 4. Change or cancel a booking |
| [Name] | 5. Write a review |
| [Name] | 6. Save trips to a wishlist |

## 5. Data model

### User

```js
const userSchema = new mongoose.Schema({
  name: { type: String, required: true },
  email: { type: String, required: true, unique: true },
  password: { type: String, required: true }, // stored hashed, never as plain text
  role: { type: String, enum: ['traveler', 'admin'], default: 'traveler' },
  savedTrips: [{ type: mongoose.Schema.Types.ObjectId, ref: 'Trip' }],
});
```

### Trip

```js
const tripSchema = new mongoose.Schema({
  title: { type: String, required: true },
  summary: { type: String, required: true },
  durationDays: { type: Number, default: 10 },
  itinerary: [{
    day: Number,
    title: String,
    description: String,
  }],
  tiers: [{ type: String, enum: ['standard', 'deluxe', 'luxury'] }],
  departureRegions: [{
    type: String,
    enum: ['Northeast', 'Southeast', 'Midwest', 'Southwest', 'West Coast'],
  }],
  active: { type: Boolean, default: true },
});
```

### Booking

```js
const bookingSchema = new mongoose.Schema({
  user: { type: mongoose.Schema.Types.ObjectId, ref: 'User', required: true },
  trip: { type: mongoose.Schema.Types.ObjectId, ref: 'Trip', required: true },
  tier: { type: String, enum: ['standard', 'deluxe', 'luxury'], required: true },
  travelers: { type: Number, min: 1, max: 12, default: 1 },
  departureRegion: { type: String, required: true },
  preferredMonth: { type: String, required: true },
  status: { type: String, enum: ['pending', 'confirmed', 'cancelled'], default: 'pending' },
}, { timestamps: true });
```

### Review

```js
const reviewSchema = new mongoose.Schema({
  user: { type: mongoose.Schema.Types.ObjectId, ref: 'User', required: true },
  trip: { type: mongoose.Schema.Types.ObjectId, ref: 'Trip', required: true },
  rating: { type: Number, min: 1, max: 5, required: true },
  comment: { type: String, required: true },
}, { timestamps: true });
```

## 6. Run it locally

### Back end

```bash
cd backend
npm install
# create backend/.env from backend/.env.example (see the table below)
npm start
```

The API runs on http://localhost:3000.

| Variable in `.env` | What it is |
|---|---|
| `MONGODB_URI` | The connection string of your MongoDB Atlas database |
| `JWT_SECRET` | Any long random text, used to sign login tokens |
| [Any other variable] | [What it is] |

### Front end

```bash
cd frontend
npm install
npm run dev
```

The app runs on [the address `npm run dev` prints, e.g. http://localhost:5173].
