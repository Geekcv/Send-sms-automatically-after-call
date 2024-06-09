# Send-sms-automatically-after-call
SMS 

Step-by-Step Guide:
Prerequisites:
1-Node.js and npm: Install from nodejs.org.
2-Twilio Account: Sign up at Twilio and get your API credentials.

Steps:
Initialize the Project:

sh
Copy code
mkdir sms-api
cd sms-api
npm init -y
npm install express twilio body-parser
Create the Express Server:
Create a file named server.js:

javascript
Copy code
const express = require('express');
const bodyParser = require('body-parser');
const twilio = require('twilio');

const app = express();
const port = process.env.PORT || 3000;

// Twilio credentials
const accountSid = 'your_twilio_account_sid';
const authToken = 'your_twilio_auth_token';
const client = new twilio(accountSid, authToken);

app.use(bodyParser.json());

// Endpoint to handle incoming requests to send SMS
app.post('/send-sms', (req, res) => {
    const { to, message } = req.body;

    client.messages.create({
        body: message,
        to: to,
        from: 'your_twilio_phone_number', // Your Twilio number
    })
    .then((message) => {
        console.log(message.sid);
        res.status(200).send('SMS sent successfully');
    })
    .catch((error) => {
        console.error(error);
        res.status(500).send('Failed to send SMS');
    });
});

app.listen(port, () => {
    console.log(`Server is running on port ${port}`);
});
Run the Server:

sh
Copy code
node server.js
Expose the Local Server (optional for testing):
Use ngrok to expose your local server to the internet for testing.

sh
Copy code
ngrok http 3000
Note the generated URL (e.g., http://abcd1234.ngrok.io).

Test the API:
You can use tools like Postman to test the API.

URL: http://localhost:3000/send-sms (or the ngrok URL)
Method: POST
Body: JSON
json
Copy code
{
  "to": "+1234567890",
  "message": "Hello, this is a test message after a call!"
}
Integrating with Call Events:
To automatically trigger this API after a call ends, you would need to handle call events on the client side (e.g., using Tasker or a custom Android app). Here’s how you could integrate it using Tasker:

Tasker Profile:

Event: Phone Offhook (call started)

Action: Set a variable %CALLINPROGRESS to 1.

Event: Phone Idle (call ended)

Action:

If %CALLINPROGRESS is 1, then send a HTTP POST request to the API endpoint.
HTTP POST Action in Tasker:

Server
: http://your_server_url/send-sms
Method: POST
Content-Type: application/json
Data/File:
json
Copy code
{
  "to": "+1234567890",
  "message": "Hello, this is a test message after a call!"
}
Variable: Clear %CALLINPROGRESS.
This setup ensures that the API is triggered automatically after the call ends, sending the SMS as needed.

