________________________________________
Project Document: HouseHunt - Finding Your Perfect Rental Home
1. Project Overview
Project Name: HouseHunt: Finding Your Perfect Rental Home
Objective: To develop a full-stack web application that facilitates the search, listing, and booking of rental properties. The system caters to three distinct user roles: Renters (Tenants), Owners, and Admin, each with specific functionalities and responsibilities.
Core Technologies (MERN Stack with Enhancements):
•	Frontend: React.js, Bootstrap, Material UI, Ant Design, Axios, Moment.js, MDB React UI Kit
•	Backend: Node.js, Express.js, MongoDB, Mongoose, JWT (JSON Web Tokens), BCrypt.js, CORS, Dotenv, Moment.js, Multer, Nodemon
•	Database: MongoDB (NoSQL)
2. Application Features and User Roles
The application is designed to provide a seamless experience for different stakeholders involved in property rental.
2.1. User Roles and Responsibilities
•	Renter (Tenant):
o	Account Management: Create an account and log in/out securely using email and password.
o	Property Discovery: Automatically view all available properties on their dashboard.
o	Detailed View: Access detailed information about a property and its owner upon clicking "Get Info."
o	Booking Request: Submit a booking request for a property through a small form, providing their details.
o	Booking Status Tracking: Monitor the status of their booking requests (initially "pending") in a dedicated "booking section," awaiting approval from the property owner.
•	Owner:
o	Admin Approval: Requires prior approval from the Admin for their owner account to become active.
o	Property Management (CRUD): Perform full Create, Read, Update, and Delete (CRUD) operations on their listed properties from their personal account dashboard.
o	Property Status Control: Manage the availability status of their properties (e.g., mark as available/unavailable).
o	Booking Management: Change the status of renter bookings (e.g., approve or reject booking requests).
•	Admin:
o	User Approval: Approve legitimate users as "Owners," granting them permission to list properties on the platform.
o	Platform Oversight: Monitor all users and activities within the application.
o	Policy Enforcement: Implement and enforce platform policies, terms of service, and privacy regulations.
3. Database Design (MongoDB Collections)
The application utilizes a NoSQL MongoDB database, structured into three primary collections.
3.1. users Collection
Stores information about all registered users (Renters, Owners, Admins).
•	_id: (ObjectId) Automatically generated unique identifier by MongoDB.
•	name: (String) Full name of the user.
•	email: (String) User's email address (unique, used for login).
•	password: (String) Hashed password for security.
•	type: (String) Defines the user's role (e.g., "Renter", "Owner", "Admin").
3.2. properties Collection
Contains details about each rental property listed by owners.
•	_id: (ObjectId) Automatically generated unique identifier.
•	userID: (ObjectId) Foreign key referencing the _id of the users collection (the owner of the property).
•	propType: (String) Type of property (e.g., "Apartment", "House", "Condo").
•	propAdType: (String) Type of advertisement (e.g., "Rent" - given the project focus).
•	isAvailable: (Boolean) Indicates if the property is currently available for booking.
•	propAddress: (String) Full address of the property.
•	ownerContact: (String) Contact information for the property owner.
•	propAmt: (Number) Rental amount/price of the property.
•	propImages: (Array of Strings) An array of URLs or paths to property images.
•	addInfo: (String) Additional descriptive information about the property.
3.3. bookings Collection
Records all booking requests made by renters.
•	_id: (ObjectId) Automatically generated unique identifier.
•	propertyId: (ObjectId) Foreign key referencing the _id of the properties collection.
•	userId: (ObjectId) Foreign key referencing the _id of the users collection (the renter who made the booking).
•	ownerId: (ObjectId) Foreign key referencing the _id of the users collection (the owner of the booked property).
•	username: (String) Name of the user who made the booking (for quick reference).
•	status: (String) Current status of the booking (e.g., "pending", "approved", "rejected").
4. Project Structure and Code Components
The project follows a standard MERN stack folder structure, separating frontend and backend concerns.
4.1. backend Folder Structure
backend/
├── config/
│   └── connect.js          // MongoDB connection configuration
├── controllers/
│   ├── adminController.js  // Logic for Admin API endpoints
│   ├── ownerController.js  // Logic for Owner API endpoints
│   └── userController.js   // Logic for User (auth, common) API endpoints
├── middlewares/
│   └── authMiddleware.js   // JWT authentication middleware
├── node_modules/           // Node.js dependencies
├── routes/
│   ├── adminRoutes.js      // API routes for Admin
│   ├── ownerRoutes.js      // API routes for Owners
│   └── userRoutes.js       // API routes for Users (auth, common)
├── schemas/
│   ├── bookingModel.js     // Mongoose schema for bookings
│   ├── propertyModel.js    // Mongoose schema for properties
│   └── userModel.js        // Mongoose schema for users
├── uploads/                // Directory for uploaded property images
├── .env                    // Environment variables (port, DB URI, JWT secret)
├── .gitignore
├── index.js                // Main server entry point
├── package-lock.json
└── package.json            // Backend dependencies (express, mongoose, bcryptjs, jsonwebtoken, etc.)
Key Backend Components:
•	index.js: Initializes the Express server, connects to MongoDB, configures middleware (CORS, body-parser), and mounts all API routes.
•	config/connect.js: Establishes the database connection using Mongoose, ensuring the server connects to MongoDB.
•	schemas/*.js: Define the structure and validation rules for each MongoDB collection using Mongoose. These models interact directly with the database.
•	controllers/*.js: Contain the business logic for handling HTTP requests. They receive requests from routes, interact with Mongoose models, and send back responses. For example, userController.js would handle user registration, login, and profile updates.
•	routes/*.js: Define the API endpoints (URLs) and link them to specific controller functions. They act as the entry points for client-side requests to the backend.
•	middlewares/authMiddleware.js: Implements JWT verification to protect routes, ensuring that only authenticated and authorized users can access certain resources.
4.2. frontend Folder Structure
frontend/
├── public/
│   └── index.html          // Main HTML file
├── src/
│   ├── images/             // Static image assets (p1.jpg, etc.)
│   ├── modules/
│   │   ├── Admin/
│   │   │   ├── AdminHome.jsx
│   │   │   ├── AllBookings.jsx
│   │   │   ├── AllProperty.jsx
│   │   │   └── AllUsers.jsx
│   │   ├── common/
│   │   │   ├── ForgotPassword.jsx
│   │   │   ├── Home.jsx
│   │   │   ├── Login.jsx
│   │   │   └── Register.jsx
│   │   └── user/
│   │       ├── Owner/
│   │       │   ├── AddProperty.jsx
│   │       │   ├── AllBookings.jsx
│   │       │   ├── AllProperties.jsx
│   │       │   └── OwnerHome.jsx
│   │       └── renter/
│   │           ├── AllProperties.jsx
│   │           └── RenterHome.jsx
│   ├── App.css             // Global styles
│   ├── App.js              // Main React component (routing, context)
│   └── index.js            // React app entry point
├── .gitignore
├── package-lock.json
└── package.json            // Frontend dependencies (react, axios, bootstrap, antd, etc.)
Key Frontend Components:
•	public/index.html: The root HTML file where the React application is injected.
•	src/index.js: Renders the main App component into the index.html.
•	src/App.js: Contains the core routing logic (using React Router, implied) to navigate between different pages/components based on the URL and user role. It might also manage global state or context.
•	src/modules/common/Login.jsx, Register.jsx: Components for user authentication, handling form inputs, and communicating with the backend API for login/registration.
•	src/modules/Admin/, src/modules/user/Owner/, src/modules/user/renter/: These directories contain role-specific React components that make up the user interfaces for each user type. For example: 
o	OwnerHome.jsx would be the dashboard for owners.
o	AddProperty.jsx would be the form for owners to add new properties.
o	AllProperties.jsx (under renter) would display all properties available to renters.
o	AllUsers.jsx (under Admin) would display all users for admin management.
•	Axios: Used extensively for making HTTP requests from the frontend to the backend API endpoints.
•	UI Libraries (Ant Design, Material UI, Bootstrap): Provide pre-built, styled components to ensure a consistent and modern user interface across the application.
5. Application Flow
The application flow typically follows these steps:
1.	User Access: A user navigates to the application (e.g., http://localhost:3000).
2.	Authentication: 
o	New users can Register by providing their name, email, password, and user type (Renter or Owner). This data is sent to the backend (/api/users/register).
o	Existing users Login with their email and password. Upon successful login, the backend returns a JWT, which is stored on the client-side (e.g., in local storage).
3.	Role-Based Redirection: Based on the user's type (obtained from the JWT or user data), the frontend redirects them to their respective dashboards (RenterHome, OwnerHome, AdminHome).
4.	Admin Workflow: 
o	Admin logs in.
o	Navigates to AllUsers.jsx to view registered users.
o	Can approve Renter users to become Owner users.
5.	Owner Workflow (After Admin Approval): 
o	Owner logs in.
o	Can navigate to AddProperty.jsx to list a new property, providing all details and uploading images.
o	Can view AllProperties.jsx to manage their listed properties (Edit/Delete).
o	Can view AllBookings.jsx to see booking requests for their properties and change their status (pending to approved/rejected).
6.	Renter Workflow: 
o	Renter logs in.
o	Sees all available properties on RenterHome.jsx (or AllProperties.jsx under renter module).
o	Clicks on a property to view Get Info which populates details and a booking form.
o	Submits their details through the form to request a booking.
o	Navigates to their booking section to view the status of their requests.
6. Project Setup and Execution Instructions
To get the project running locally, follow these steps:
6.1. Prerequisites
•	Node.js and npm: Installed on your machine. (Download from https://nodejs.org/)
•	MongoDB: A running MongoDB instance (local or cloud-hosted like MongoDB Atlas). (Download Community Server from https://www.mongodb.com/try/download/community)
•	Code Editor: VS Code or similar.
6.2. Download the Code
•	Download the project ZIP file from the provided Google Drive link: https://drive.google.com/drive/folders/10OstrbGEPKtXDENGkerKBShejPJIbVgj?usp=sharing
•	Extract the contents to a desired location on your computer.
6.3. Backend Setup
1.	Open your terminal or command prompt.
2.	Navigate to the backend directory: 
Bash
cd path/to/extracted/folder/backend
3.	Install backend dependencies: 
Bash
npm install
4.	Create a .env file in the backend directory and add your environment variables. Example: 
5.	PORT=5000
6.	MONGO_URI=mongodb://localhost:27017/househunt_db
7.	JWT_SECRET=your_jwt_secret_key_here
o	Note: Replace your_jwt_secret_key_here with a strong, random string. Adjust MONGO_URI if your MongoDB instance is different.
8.	Start the backend server: 
Bash
npm start
o	The server should start on the specified port (e.g., http://localhost:5000).
6.4. Frontend Setup
1.	Open a new terminal or command prompt window.
2.	Navigate to the frontend directory: 
Bash
cd path/to/extracted/folder/frontend
3.	Install frontend dependencies: 
Bash
npm install
4.	Start the frontend development server: 
Bash
npm start
o	The React application will usually open automatically in your browser at http://localhost:3000.
6.5. Verification
•	Once both servers are running, access http://localhost:3000 in your web browser.
•	Test the Register and Login functionalities.
•	Create an Owner account, then manually change its type in MongoDB to Admin for testing admin functionalities (or implement an initial admin creation route).
•	Approve the Owner account via the Admin panel.
•	Test property listing as an Owner.
•	Create a Renter account and test property Browse and booking.

