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
  ```
