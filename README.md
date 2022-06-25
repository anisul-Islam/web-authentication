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
    encryptionKey: encKey,
    encryptedFields: ["age"],
  });

  User = mongoose.model("User", userSchema);
  ```
