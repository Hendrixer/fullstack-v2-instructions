## Passwords

Before we can create the API routes to handle user auth, we need some utility functions.

Create a new file, `auth` in the `lib` folder and add this code. These functions will help us hash and check passwords.

```ts
import bcrypt from "bcrypt";
import { SignJWT, jwtVerify } from "jose";
import { db } from "./db";

export const hashPassword = (password) => bcrypt.hash(password, 10);

export const comparePasswords = (plainTextPassword, hashedPassword) =>
  bcrypt.compare(plainTextPassword, hashedPassword);
```

**Note:** If you are adding types to your code, you'll need to install @types/bcrypt:

```bash
`npm i -D @types/bcrypt`
```

This file might be imported by a server component so we'll tell Next.js about the dependencies In the `next.config.js` we'll add `bcrypt`:

```js
experimental: {
  appDir: true,
  serverComponentsExternalPackages: ['bcrypt'],
},
```
