# Send-sms-automatically-after-call
SMS 

Step-by-Step Guide:
Prerequisites:<BR>
1-Node.js and npm: Install from nodejs.org.<BR>
2-Twilio Account: Sign up at Twilio and get your API credentials.<BR><BR><BR>

<h1>code</h1>
const express = require('express');<BR>
const bodyParser = require('body-parser');<BR>
const twilio = require('twilio');<BR><BR>

const app = express();<BR>
const port = process.env.PORT || 3000;<BR><BR><BR>

// Twilio credentials<BR>
const accountSid = 'your_twilio_account_sid';<BR>
const authToken = 'your_twilio_auth_token';<BR>
const client = new twilio(accountSid, authToken);<BR><BR><BR>

app.use(bodyParser.json());<BR><BR>

// Endpoint to handle incoming requests to send SMS<BR>
app.post('/send-sms', (req, res) => {<BR>
    const { to, message } = req.body;<BR>

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
});<BR>

app.listen(port, () => {<BR>
    console.log(`Server is running on port ${port}`);<BR>
});<br>


<h1>package</h1>
npm install express twilio body-parser


<h1>test api</h1>

URL: http://localhost:3000/send-sms (or the ngrok URL)<br>
Method: POST<br>
Body: JSON<br>
json<br>
Copy code<br>
{<br>
  "to": "+1234567890",<br>
  "message": "Hello, this is a test message after a call!"<br>
}<br>




