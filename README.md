<!DOCTYPE html>
<html>
<head>
  <title>Company Mailer</title>
</head>
<body>
  <h1>Company Mailer</h1>
  <form id="email-form">
    <label for="to">Whom:</label>
    <input type="email" id="to" name="to" required><br><br>

    <label for="subject">Тема:</label>
    <input type="text" id="subject" name="subject" required><br><br>

    <label for="message">Message:</label>
    <textarea id="message" name="message" required></textarea><br><br>

    <button type="submit">Send an email</button>
  </form>

  <div id="message-status"></div>

  <script src="script.js"></script>
</body>
</html>

// Replace these values with your SendGrid API keys
const API_KEY = 'YOUR_SENDGRID_API_KEY'; 
const API_URL = 'https://api.sendgrid.com/v3/mail/send'; 

// Function for sending a letter
function sendEmail(to, subject, message) {
  const data = {
    personalizations: [
      {
        to: [{ email: to }]
      }
    ],
    from: {
      email: 'your_sender_email@example.com' // Your sender's address
    },
    subject: subject,
    content: [
      {
        type: 'text/plain',
        value: message
      }
    ]
  };

  fetch(API_URL, {
    method: 'POST',
    headers: {
      'Authorization': Bearer ${API_KEY},
      'Content-Type': 'application/json'
    },
    body: JSON.stringify(data)
  })
  .then(response => {
    if (!response.ok) {
      throw new Error('Network response error');
    }
    return response.json();
  })
  .then(data => {
    displayMessage('The email has been sent successfully!');
    console.log(data); // Additional information from the API
  })
  .catch(error => {
    displayMessage(Error sending the email: ${error.message});
    console.error(error);
  });
}

// A function for displaying success/error messages
function displayMessage(message) {
  const messageStatus = document.getElementById('message-status');
  messageStatus.textContent = message;
}

// Event handler for sending the form
const emailForm = document.getElementById('email-form');
emailForm.addEventListener('submit', (event) => {
  event.preventDefault(); // Prevent standard form submission
  const to = document.getElementById('to').value;
  const subject = document.getElementById('subject').value;
  const message = document.getElementById('message').value;

  sendEmail(to, subject, message);
});
