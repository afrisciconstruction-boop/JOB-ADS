# Setup Guide for JOB-ADS Repository

## Step 1: Setting up the Backend
1. Clone the repository:
   ```bash
   git clone https://github.com/afrisciconstruction-boop/JOB-ADS.git
   cd JOB-ADS
   ```
2. Install dependencies:
   ```bash
   cd backend
   npm install
   ```
3. Configure environment variables by creating a `.env` file in the `backend` directory. Use the `.env.example` as a reference.

## Step 2: Setting up the Frontend
1. Navigate to the frontend directory:
   ```bash
   cd ../frontend
   ```
2. Install dependencies:
   ```bash
   npm install
   ```
3. Start the frontend:
   ```bash
   npm start
   ```

## Step 3: Setting up the Mobile App
1. Navigate to the mobile app directory:
   ```bash
   cd ../mobile
   ```
2. Install dependencies:
   ```bash
   npm install
   ```
3. Run the mobile app:
   ```bash
   npm start
   ```

## Step 4: Setting up the Database
1. Ensure you have the required database server installed (e.g., MySQL, MongoDB).
2. Create a new database for the application.
3. Run the migrations:
   ```bash
   cd backend
   npm run migrate
   ```

## Step 5: Setting up Environment Variables
- Create a `.env` file in both the `backend` and `frontend` directories.
- Add the required variables. Refer to `.env.example` for guidance.

## Step 6: Docker Setup
1. Ensure Docker is installed on your machine.
2. Build the Docker containers:
   ```bash
   docker-compose build
   ```
3. Run the containers:
   ```bash
   docker-compose up
   ```

## Step 7: Local Development
- Use your preferred code editor to work on the project.
- Ensure that the backend and frontend servers are running while developing.

---

This guide provides a structured approach to setting up the JOB-ADS repository for local development. Follow each step carefully to ensure a successful setup.