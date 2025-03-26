# Email Verification System

This project implements an email verification system using a front-end form to input the user's email address and verification code. The form sends the email to a backend API to send a verification code. Once the user receives the code, they can enter it to verify the email.

## Features

- **Email Input:** Allows the user to enter their email address.
- **Verification Code:** Sends a verification code to the entered email.
- **Code Input:** After receiving the code, the user enters it to complete the verification process.
- **Backend Communication:** The app communicates with a backend to send and verify the code.

## Tech Stack

- **HTML**: Structure of the email verification form.
- **CSS**: Basic styling using Google Fonts and custom styles.
- **JavaScript**: Logic to handle sending the verification code and verifying it.
- **Backend (not included)**: Requires a backend API to send and verify the code (e.g., a server running on `localhost:3000`).

## Prerequisites

- A backend API that listens on `http://localhost:3000` for POST requests at the following endpoints:
  - `/send-verification`: Sends a verification code to the given email.
  - `/verify-code`: Verifies the code provided by the user.

## Setup

### Frontend Setup

1. Clone this repository to your local machine:
    ```bash
    git clone https://github.com/yourusername/email-verification.git
    cd email-verification
    ```

2. Open the `index.html` file in a browser.

### Backend Setup (Optional)

This frontend requires a backend to handle the verification process. Below is a basic setup using Node.js and Express:

1. Create a new directory for the backend:
    ```bash
    mkdir backend
    cd backend
    npm init -y
    npm install express nodemailer body-parser
    ```

2. Create a simple server (`server.js`) in the `backend` directory:

    ```javascript
    const express = require('express');
    const nodemailer = require('nodemailer');
    const bodyParser = require('body-parser');
    const app = express();
    const PORT = 3000;

    app.use(bodyParser.json());

    // Mock database to store codes (for demonstration purposes)
    let codes = {};

    // Endpoint to send verification code
    app.post('/send-verification', (req, res) => {
        const { email } = req.body;
        const code = Math.floor(100000 + Math.random() * 900000); // Generate a random 6-digit code

        // Store the code (in a real application, you'd send it via email)
        codes[email] = code;

        console.log(`Sending code ${code} to ${email}`);

        // You would integrate with an email service here to send the code via email (e.g., Nodemailer)
        res.json({ message: `Verification code sent to ${email}.` });
    });

    // Endpoint to verify the code
    app.post('/verify-code', (req, res) => {
        const { email, code } = req.body;

        if (codes[email] === parseInt(code)) {
            res.json({ message: 'Email verified successfully!' });
        } else {
            res.json({ message: 'Invalid verification code.' });
        }
    });

    app.listen(PORT, () => {
        console.log(`Server is running on http://localhost:${PORT}`);
    });
    ```

3. Start the backend server:
    ```bash
    node server.js
    ```

### Running the Project

1. Make sure the backend server is running.
2. Open the `index.html` file in a browser.
3. Enter your email and click "Send Code."
4. Enter the code received to verify your email.

## How It Works

1. The user enters their email address and clicks "Send Code."
2. The frontend sends a request to the backend API to send the verification code to the email.
3. Once the user receives the code, they enter it into the form and click "Verify."
4. The frontend sends the entered code to the backend to verify it.
5. If the code is correct, the user is redirected to a success page.

## License

This project is open-source and available under the [MIT License](LICENSE).
