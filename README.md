
## Installation
```
npm init -y
npm install express mongoose nodemon dotenv
```
## .env
Connexion à mongoDB
```
DATABASE_URL = 
```
## Schema
models.js
```javascript
const mongoose = require('mongoose');

const userSchema = new mongoose.Schema({
  name: String,
  email:String,
  age: Number,
});

module.exports = mongoose.model('User', userSchema);
```
## Routers
user.js
```javascript
const express = require('express');
const router = express.Router();
const User = require('./models');

// Get all users
router.get('/', async (req, res) => {
  try {
    const users = await User.find();
    res.json(users);
  } catch (err) {
    res.status(500).json({ message: err.message });
  }
});

// Get a single user
router.get('/:id', getUser, (req, res) => {
  res.json(res.user);
});

// Create a new user
router.post('/', async (req, res) => {
  const user = new User({
    name: req.body.name,
    email: req.body.email,
    age: req.body.age
  });

  try {
    const newUser = await user.save();
    res.status(201).json(newUser);
  } catch (err) {
    res.status(400).json({ message: err.message });
  }
});

// Update a user
router.put('/:id', getUser, async (req, res) => {
  if (req.body.name != null) {
    res.user.name = req.body.name;
  }

  if (req.body.email != null) {
    res.user.email = req.body.email;
  }

  if (req.body.age != null) {
    res.user.age = req.body.age;
  }

  try {
    const updatedUser = await res.user.save();
    res.json(updatedUser);
  }catch (err) {
    res.status(400).json({ message: err.message });
    }
    });
    
    // Delete a user
    router.delete('/:id', getUser, async (req, res) => {
    try {
    await res.user.remove();
    res.json({ message: 'User deleted' });
    } catch (err) {
    res.status(500).json({ message: err.message });
    }
    });

    
    
    async function getUser(req, res, next) {
    let user;
    try {
    user = await User.findById(req.params.id);
    if (user == null) {
    return res.status(404).json({ message: 'Cannot find user' });
    }
    } catch (err) {
    return res.status(500).json({ message: err.message });
    }
    
    res.user = user;
    next();
    }
    
    module.exports = router;
```


## Main
index.js
```javascript
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
```
