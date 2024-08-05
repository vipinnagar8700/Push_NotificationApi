const express = require('express');
const bodyParser = require('body-parser');
const admin = require('./firebase-admin-setup');

const app = express();
const port = 3000;

app.use(bodyParser.json());

app.post('/send-notification', async (req, res) => {
  const { token, message } = req.body;

  const payload = {
    notification: {
      title: 'New Notification',
      body: message,
    },
    data: {
      screen: 'NotificationScreen'  // Custom data for redirection
    }
  };

  try {
    const response = await admin.messaging().sendToDevice(token, payload);
    res.status(200).send(response);
  } catch (error) {
    console.error('Error sending notification:', error);
    res.status(500).send(error);
  }
});

app.listen(port, () => {
  console.log(`Server is running on port ${port}`);
});
// index.js
const admin = require('firebase-admin');
const serviceAccount = require('./google-services.json');

admin.initializeApp({
  credential: admin.credential.cert(serviceAccount)
});

module.exports = admin;
//Firebase initialise
