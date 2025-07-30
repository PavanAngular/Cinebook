# CineBook - Movie Ticket Booking System

CineBook is a modern, responsive web application for booking movie tickets online. Built with HTML, CSS, and JavaScript, it leverages Firebase Firestore as its backend for real-time data management, including dynamic movie listings, theatre information, and real-time seat availability and blocking.

## ✨ Features

* **City Selection:** Users can select their preferred city to find movies playing nearby.
* **Dynamic Movie Listings:** Displays movies currently playing in the selected city, complete with poster images.
* **Theatre Selection:** Filter and select theatres based on the chosen movie.
* **Showtime & Screen Selection:** Choose specific showtimes and screens available for a movie at a selected theatre.
* **Real-time Seat Selection & Blocking:**
    * Users can interactively select available seats.
    * Selected seats are temporarily "pending" in real-time, blocking them for other users during the booking process.
    * Permanently "booked" seats are clearly marked and unavailable.
* **Payment Simulation:** A simple payment summary showing the total amount for selected seats.
* **Booking Confirmation:** Displays detailed booking information (movie, theatre, time, seats, total paid) upon successful confirmation.
* **Navigation:** Intuitive step-by-step flow with "Back" buttons for easy navigation.
* **Firebase Integration:** Utilizes Firestore for robust data storage and real-time updates.

## 🚀 Technologies Used

* **Frontend:**
    * HTML5 (Structure)
    * CSS3 (Styling)
    * JavaScript (Client-side Logic)
* **Backend & Database:**
    * **Firebase Firestore:** NoSQL cloud database for storing movie, theatre, showtime, seat, and booking data.
* **Firebase SDK (Compat Mode):** Used for interacting with Firebase services from the frontend.
* **Placeholder.it:** For reliable placeholder movie poster images during development/demonstration.

## 📦 Setup and Local Installation

Follow these steps to get CineBook running on your local machine:

### Prerequisites

1.  **Web Browser:** Chrome, Firefox, Edge, etc.
2.  **Text Editor:** VS Code, Sublime Text, etc.
3.  **Firebase Project:**
    * Go to [Firebase Console](https://console.firebase.google.com/).
    * Create a new Firebase project.
    * Register a new **web app** within your project. Copy the `firebaseConfig` object provided by Firebase.
    * Enable **Firestore Database** in your Firebase project (start in production mode).
    * **Crucially, set up Firestore Security Rules for development:**
        * Go to Firestore Database -> Rules tab.
        * Replace existing rules with this (for temporary development **ONLY**, highly insecure for production):
            ```firestore
            rules_version = '2';
            service cloud.firestore {
              match /databases/{database}/documents {
                match /{document=**} {
                  allow read, write: if true;
                }
              }
            }
            ```
        * **Click "Publish Rules" after making changes.**
    * **Create a Composite Index:** Your showtimes query requires a composite index.
        * Go to Firestore Database -> Indexes tab.
        * Click "Add Index" and create the following:
            * **Collection ID:** `showtimes`
            * **Fields:**
                1.  `theatre_id` (Ascending)
                2.  `movie_id` (Ascending)
                3.  `time` (Ascending)
            * **Click "Create".** Wait for the index to finish building.

### Installation Steps

1.  **Clone or Download Project:**
    * Download the `index.html` file (and any associated external CSS/JS if you move them outside the HTML file).
    * Place `index.html` in a folder on your computer (e.g., `C:\Users\YourUser\Desktop\CineBookApp\`).

2.  **Configure Firebase in `index.html`:**
    * Open your `index.html` file in your text editor.
    * Locate the `<script>` block containing `const firebaseConfig = { ... };`.
    * **Replace the placeholder `apiKey`, `authDomain`, `projectId`, etc., with the exact values you copied from your Firebase Console.**

3.  **Clear Existing Firestore Data (IMPORTANT for Initial Population):**
    * Go to your Firebase Console -> Firestore Database -> Data tab.
    * **Delete ALL documents from the `cities`, `movies`, `theatres`, `showtimes`, and `bookings` collections.** This ensures `populateInitialData()` runs and creates fresh, correctly linked data.

4.  **Run the Application:**
    * Open the `index.html` file directly in your web browser.
    * Open your browser's Developer Tools (usually by pressing `F12`). Go to the "Console" tab.
    * **Perform a Hard Refresh** (`Ctrl + Shift + R` on Windows/Linux, `Cmd + Shift + R` on macOS).
    * **Observe the Console:** You should see messages indicating Firebase initialization and data population (`No data found. Populating initial data...`, followed by "Added city/movie/theatre..." messages, and finally `Initial data populated successfully!`). This means your Firestore is now populated with sample data.

## 🎯 Usage

1.  **Select City:** Click on a city button (e.g., "Hyderabad").
2.  **Browse Movies:** The available movies for that city will be displayed with placeholder posters. Click on a movie poster to select it.
3.  **Choose Theatre:** Select a theatre where the chosen movie is playing.
4.  **Select Showtime:** Pick a screen and showtime from the available options.
5.  **Pick Seats:** Click on the seat buttons to select your desired seats. Selected seats will turn green. Unavailable (booked/pending) seats will be red/orange and disabled. The total amount will update.
6.  **Proceed to Payment:** Click the "Proceed to Payment" button. This will temporarily block your selected seats in Firestore.
7.  **Confirm Booking:** Review the total amount and click "Confirm Booking". This will finalize the booking in Firestore, marking seats as "booked" and creating a booking record.
8.  **Booking Confirmation:** View your booking details, including movie poster, theatre, time, and seats.
9.  **Book Another Ticket:** Click this button to restart the booking process.

## 🚀 Deployment (Sharing Your Project)

Since your project runs entirely on the client-side with Firebase as the backend, you can easily host it on static site hosting services.

**Using GitHub Pages (as chosen):**

1.  **Initialize Git & Commit:** If not already done, in your project folder (where `index.html` is), run:
    ```bash
    git init
    git add .
    git commit -m "Initial CineBook project"
    ```
2.  **Create GitHub Repository:** Go to [github.com](https://github.com/), create a **new public repository** (e.g., `CineBook`).
3.  **Link Local to Remote & Push:**
    ```bash
    git remote add origin [https://github.com/YOUR_USERNAME/CineBook.git](https://github.com/YOUR_USERNAME/CineBook.git)
    git branch -M main
    git push -u origin main
    ```
    (Replace `YOUR_USERNAME` and `CineBook` with your actual details).
4.  **Enable GitHub Pages:**
    * On your GitHub repository, go to **Settings > Pages**.
    * Under "Build and deployment", select **"Deploy from a branch"**.
    * For "Branch", select `main` (or `master`) and choose the **`/ (root)`** folder.
    * Click **"Save"**.
5.  **Get Link:** Wait a minute or two. Refresh the Pages settings, and your shareable URL will appear (e.g., `https://YOUR_USERNAME.github.io/CineBook/`).

## 🔮 Future Enhancements (Ideas)

* **User Authentication:** Implement Firebase Authentication for user accounts, allowing users to view past bookings.
* **Payment Gateway Integration:** Integrate with a real payment gateway (e.g., Stripe, Razorpay) using Firebase Cloud Functions for secure transactions.
* **Timeout for Pending Seats:** Implement a Firebase Cloud Function to automatically release seats that are "pending" after a certain time limit if the booking isn't confirmed.
* **Admin Panel:** Create a separate interface for theatre owners or administrators to manage movies, showtimes, and seat layouts.
* **Responsive Design:** Further optimize for various screen sizes (beyond basic responsiveness).
* **Search & Filters:** Add search functionality for movies, and filters for showtimes (e.g., by genre, time of day).
* **Notifications:** Implement email or in-app notifications for booking confirmations.

## 🤝 Contributing

Feel free to fork this repository, make improvements, and submit pull requests.

## 📄 License

This project is open source and available under the MIT License.

---
