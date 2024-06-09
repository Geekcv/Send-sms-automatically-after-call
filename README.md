# Send-sms-automatically-after-call-End


<h1>Prerequisites:</h1><BR>
1-Node.js <BR>
2-Twilio Account: Sign up at Twilio and get your API credentials.<BR><BR><BR>

<h1>Package</h1>
npm install express twilio body-parser


<h1>Code</h1>
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




<h1>Test API In Postman</h1>

URL: http://localhost:3000/send-sms (or the ngrok URL)<br>
Method: POST<br>
Body: JSON<br>
json<br>
{<br>
  "to": "+917297028304",<br>
  "message": "Hello, this is a test message after a call!"<br>
}<br>

<h1>Integrating with Call Events</h1>
Tasker Profile:<br>
Event: Phone Offhook (call started)<br>
Action: Set a variable %CALLINPROGRESS to 1.<br>
Event: Phone Idle (call ended)<br>

Action:<br>
If %CALLINPROGRESS is 1, then send a HTTP POST request to the API endpoint.<br>

HTTP POST Action in Tasker:<br>
Server URL<br>
: http://your_server_url/send-sms<br>
Method: POST<br>
Content-Type: application/json<br>
Data/File:<br>
json<br>
{<br>
  "to": "+917297028304",<br>
  "message": "Hello, this is a test message after a call!"<br>
}<br>
Variable: Clear %CALLINPROGRESS.<br>

<H2>This setup ensures that the API is triggered automatically after the call ends, sending the SMS as needed.</H2>



