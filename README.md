# Send-sms-automatically-after-call
SMS 

Step-by-Step Guide:
Prerequisites:<BR>
1-Node.js and npm: Install from nodejs.org.<BR>
2-Twilio Account: Sign up at Twilio and get your API credentials.<BR><BR><BR>

<h1>code</h1>
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


