## Level 1: Database matching

- save(), find({property: value})
- if hacker can access our database then our data is too much human readable

## Level 2: Database Encryption

- read mongoose encryption documentation: https://www.npmjs.com/package/mongoose-encryption
- install mongoose encryption `npm install mongoose-encryption`
- create new mongoose Schema

  ```js
  const mongoose = require("mongoose");
  const encrypt = require("mongoose-encryption");

  const userSchema = new mongoose.Schema({
    name: String,
    age: Number,
    // whatever else
  });
  ```

- create an encryption key inside .env file

  ```js
  ENCRYPTION_KEY = thisismyencryptionkey;
  ```

- set encryption key with our schema

  ```js
  const encrypt = require("mongoose-encryption");

  const encKey = process.env.ENCRYPTION_KEY;
  // encrypt age regardless of any other options. name and _id will be left unencrypted
  userSchema.plugin(encrypt, {
    secret: encKey,
    encryptedFields: ["age"],
  });

  User = mongoose.model("User", userSchema);
  ```

## Level 3: Hashing password

- no cncryption key; we will use hashing algorithm
- hackers can not convert to plain text as no encryption key is available
- md5 package: https://www.npmjs.com/package/md5
- install md5 npm package: `npm install md5`
- usage

  ```js
  var md5 = require("md5");
  console.log(md5("message"));
  // 78e731027d8fd50ed642340b7c9a63b3

  // hash password when create it
  const newUser = new User({
    email: req.body.username,
    password: md5(req.body.password),
  });

  app.post("/login", async (req, res) => {
    try {
      const email = req.body.email;
      const password = md5(req.body.password);
      const user = await User.findOne({ email: email });
      if (user && user.password === password) {
        res.status(200).json({ status: "valid user" });
      } else {
        res.status(404).json({ status: "Not valid user" });
      }
    } catch (error) {
      res.status(500).json(error.message);
    }
  });
  ```

## Level 4: Hashing + salting password

- we can hash the password with some random number(salting)
- install bcrypt npm package `npm install bcrypt`
- usage

  ```js
  const bcrypt = require("bcrypt");
  const saltRounds = 10;

  app.post("/register", async (req, res) => {
    try {
      bcrypt.hash(req.body.password, saltRounds, async function (err, hash) {
        const newUser = new User({
          email: req.body.email,
          password: hash,
        });
        await newUser.save();
        res.status(201).json(newUser);
      });
    } catch (error) {
      res.status(500).json(error.message);
    }
  });

  app.post("/login", async (req, res) => {
    try {
      const email = req.body.email;
      const password = req.body.password;
      const user = await User.findOne({ email: email });
      if (user) {
        bcrypt.compare(password, user.password, function (err, result) {
          if (result === true) {
            res.status(200).json({ status: "valid user" });
          }
        });
      } else {
        res.status(404).json({ status: "Not valid user" });
      }
    } catch (error) {
      res.status(500).json(error.message);
    }
  });
  ```
