The fastest and easiest way to setup a Psql DB if you don't already have one is to use a hosted one.

Checkout [Railway](https://railway.app/) to setup a free one. Either way, you'll need the connection string. Inside your `.env` file, add your DB connection string

```bash
DATABASE_URL="your-connection-string"
```

<br>

We can then initialize a prisma project with:
`npx prisma init`
