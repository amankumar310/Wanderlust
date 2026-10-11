# 🌍 Wanderlust

**Wanderlust** is a full-stack travel accommodation listing web application inspired by Airbnb. Users can explore places to stay, view listing details and locations on a map, create accounts, publish listings with images, and share reviews.

The application is built with Node.js, Express, MongoDB, EJS, Passport.js, Cloudinary, and Mapbox.

## ✨ Features

- **Browse listings:** View available accommodation listings and their details.
- **Listing management:** Authenticated users can create listings, upload images, edit their own listings, and delete their own listings.
- **User authentication:** Register, log in, and log out using Passport Local.
- **Reviews and ratings:** Logged-in users can submit reviews with a rating from 1 to 5 stars.
- **Authorization:** Middleware checks listing ownership and review authorship before allowing protected changes.
- **Image uploads:** Store listing images using Cloudinary and Multer.
- **Interactive maps:** Display a listing location with Mapbox maps and geocoding.
- **Input validation:** Validate listing and review data with Joi.
- **Flash messages:** Show success and error messages to users.
- **Reusable page layouts:** Render server-side pages using EJS and EJS-Mate.
- **Persistent sessions:** Store login sessions in MongoDB using `connect-mongo`.

## 🧰 Tech Stack

| Area | Technologies |
|---|---|
| Runtime | Node.js |
| Server | Express.js 5 |
| Database | MongoDB, Mongoose |
| Views | EJS, EJS-Mate |
| Authentication | Passport.js, Passport Local, Passport Local Mongoose |
| Sessions | Express Session, Connect Mongo |
| Image storage | Cloudinary, Multer, Multer Storage Cloudinary |
| Maps and geocoding | Mapbox SDK, Mapbox GL JS |
| Validation | Joi |
| UI | HTML, CSS, Bootstrap-based templates, JavaScript |
| Configuration | dotenv |
| Notifications | connect-flash |

## 📁 Project Structure

```text
Wanderlust/
├── app.js                       # Express application setup and server
├── cloudConfig.js                # Cloudinary and upload-storage setup
├── middleware.js                 # Authentication, authorization, validation
├── schema.js                     # Joi schemas for listings and reviews
├── package.json                  # Project metadata and dependencies
├── package-lock.json             # Locked dependency versions
├── controllers/
│   ├── listings.js               # Listing operations and geocoding
│   ├── reviews.js                # Review creation and deletion
│   └── users.js                  # Signup, login, and logout handlers
├── models/
│   ├── listing.js                # Listing schema and review cleanup hook
│   ├── reviews.js                # Review schema
│   └── user.js                   # User schema and Passport integration
├── routes/
│   ├── listing.js                # Listing routes
│   ├── review.js                 # Review routes
│   └── user.js                   # Authentication routes
├── init/
│   ├── data.js                   # Sample listing data
│   └── index.js                  # Database seed script
├── public/
│   ├── css/
│   │   ├── style.css             # Main styles
│   │   └── rating.css            # Star-rating styles
│   └── js/
│       ├── map.js                # Mapbox map and marker
│       └── script.js             # Client-side interactions
├── utils/
│   ├── ExpressError.js           # Custom HTTP error class
│   └── wrapAsync.js               # Async route error wrapper
└── views/
    ├── error.ejs                 # Error page
    ├── includes/
    │   ├── flash.ejs              # Flash message partial
    │   ├── footer.ejs             # Footer partial
    │   └── navbar.ejs             # Navigation partial
    ├── layouts/
    │   └── boilerplate.ejs        # Shared page layout
    ├── listings/
    │   ├── index.ejs              # All listings
    │   ├── show.ejs               # Listing details, reviews, map
    │   ├── new.ejs                # Create listing form
    │   └── edit.ejs               # Edit listing form
    └── users/
        ├── signup.ejs             # Registration page
        └── login.ejs              # Login page
```

## ⚙️ Prerequisites

Install the following before running the application:

- [Node.js](https://nodejs.org/) and npm. The current `package.json` specifies Node.js `23.9.0`.
- A MongoDB database, either a local MongoDB server or a MongoDB Atlas cluster.
- A [Cloudinary](https://cloudinary.com/) account for image uploads.
- A [Mapbox](https://www.mapbox.com/) account and access token for geocoding and maps.
- Git, to clone the repository.

## 🚀 Installation and Setup

### 1. Clone the repository

```bash
git clone https://github.com/amankumar310/Wanderlust.git
cd Wanderlust
```

### 2. Install dependencies

```bash
npm install
```

### 3. Configure environment variables

Create a file named `.env` in the project root, alongside `app.js`.

```env
# MongoDB connection string
ATLASDB_URL=mongodb+srv://<username>:<password>@<cluster-url>/wanderlust

# Express session and MongoDB session-store encryption secret
SECRET=replace_with_a_long_random_secret

# Cloudinary credentials
CLOUD_NAME=your_cloudinary_cloud_name
CLOUD_API_KEY=your_cloudinary_api_key
CLOUD_API_SECRET=your_cloudinary_api_secret

# Mapbox access token used for geocoding and maps
MAP_TOKEN=your_mapbox_access_token

# Environment (optional for local development)
NODE_ENV=development
```

Replace the placeholder values with your own credentials. If using MongoDB Atlas, make sure your database user and network access settings allow your application to connect.

**Important:** Never commit your `.env` file or publish API secrets. Keep credentials private and use a strong, unique value for `SECRET`.

### 4. Start the application

The current `package.json` does not define a start script, so run the entry point directly:

```bash
node app.js
```

For automatic restarts during development, you can use a tool such as Nodemon:

```bash
npx nodemon app.js
```

The application listens on **port `8080`**. Open:

- Listings page: <http://localhost:8080/listings>
- Signup page: <http://localhost:8080/signup>
- Login page: <http://localhost:8080/login>

Keep the terminal running while using the application.

## 🗃️ Database Initialization (Optional)

The repository includes sample data in `init/data.js` and a seed script in `init/index.js`.

Before running the seed script, review `init/index.js`: it currently connects to a local database at `mongodb://127.0.0.1:27017/wanderlust` and assigns a hard-coded owner ID to the sample listings. Make sure that the database URL and owner ID match your setup before running it.

If your local MongoDB server is running and the seed script is correctly configured, run:

```bash
node init/index.js
```

**Warning:** The seed script calls `Listing.deleteMany({})`, which deletes existing listing documents before inserting sample data. Back up any data you need to keep before running it.

## 🧭 How It Works

1. Express receives a request and sends it to the relevant router.
2. Middleware checks authentication, ownership, and submitted data where required.
3. Controllers perform application logic and interact with MongoDB through Mongoose models.
4. Cloudinary stores uploaded listing images.
5. Mapbox geocoding converts a listing's location into coordinates, which are saved with the listing.
6. EJS templates render listing pages, authentication forms, reviews, and map details.
7. Passport Local manages authentication, while Express Session and MongoDB persist login sessions.

## 🔗 Main Routes

| Method | Route | Purpose | Access |
|---|---|---|---|
| `GET` | `/listings` | View all listings | Public |
| `GET` | `/listings/new` | Open the create-listing form | Logged in |
| `POST` | `/listings` | Create a listing and upload its image | Logged in |
| `GET` | `/listings/:id` | View a listing, reviews, and map | Public |
| `GET` | `/listings/:id/edit` | Open the edit form | Listing owner |
| `PUT` | `/listings/:id` | Update a listing | Listing owner |
| `DELETE` | `/listings/:id` | Delete a listing | Listing owner |
| `POST` | `/listings/:id/reviews` | Add a review | Logged in |
| `DELETE` | `/listings/:id/reviews/:reviewId` | Delete a review | Review author |
| `GET` | `/signup` | Open the registration page | Public |
| `POST` | `/signup` | Register a user | Public |
| `GET` | `/login` | Open the login page | Public |
| `POST` | `/login` | Authenticate a user | Public |
| `GET` | `/logout` | Log out | Authenticated session |

The update and delete listing routes use HTTP method override so HTML forms can submit `PUT` and `DELETE` requests.

## 🔐 Security and Validation

- Passport Local handles username/password authentication.
- Passport Local Mongoose manages user authentication data.
- Express sessions are stored in MongoDB.
- Listing changes are restricted to the listing owner.
- Review deletion is restricted to the review author.
- Joi validates listing and review form data.
- Session cookies use the `httpOnly` option.

These controls are part of the application, but they do not replace a full production security review. Before deploying, review session-cookie settings, error handling, rate limiting, and all authorization checks.

## 🛠️ Troubleshooting

- **MongoDB connection error:** Check `ATLASDB_URL`, database credentials, Atlas network access, and database availability.
- **Image upload fails:** Verify `CLOUD_NAME`, `CLOUD_API_KEY`, and `CLOUD_API_SECRET`.
- **Map does not load:** Verify `MAP_TOKEN` and confirm the browser map library is loaded in the listing page layout.
- **Application cannot find credentials:** Ensure `.env` is in the project root and restart the Node.js process after editing it.
- **Port 8080 is busy:** Stop the other process using port 8080. The current application code uses a fixed port rather than reading `PORT` from the environment.
- **Sample data causes an owner error:** Update the owner ID in `init/index.js` to a valid user ID before seeding.

## 🧪 Tests

A test suite is not currently configured in `package.json`. The existing `npm test` command intentionally exits with a “no test specified” message.

## 🔮 Possible Improvements

- Add automated unit and integration tests.
- Add search, filtering, sorting, and pagination.
- Improve validation and error handling for missing or invalid Mapbox results.
- Add deployment instructions and production environment configuration.
- Add a `.env.example` file containing placeholders only.
- Configure production-safe session cookies and security middleware.
- Add a configurable port and a standard `npm start` script.

## 👨‍💻 Author

**Aman Kumar**

- GitHub: [@amankumar310](https://github.com/amankumar310)
- Project repository: [Wanderlust](https://github.com/amankumar310/Wanderlust)

## 📄 License

The repository currently declares the `ISC` license in `package.json`. Add a `LICENSE` file to the repository if you intend to distribute the project under that license.
