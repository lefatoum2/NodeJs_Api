const express = require('express');
const mongoose = require('mongoose');


// Connection Database 
require('dotenv').config();
const DBString = process.env.DATABASE_URL
mongoose.connect(DBString);
const database = mongoose.connection

database.on('error', (error) => {
    console.log(error)
})

database.once('connected', () => {
    console.log('Database Connected');
})


// Set up the express app
const app = express();

// Allows us to accept the data in JSON format
app.use(express.json());

// Start the server
const port = process.env.PORT || 3000;
app.listen(port, () => console.log(`Server started on port ${port}`));

const usersRouter = require('./user');
app.use('/users', usersRouter);
