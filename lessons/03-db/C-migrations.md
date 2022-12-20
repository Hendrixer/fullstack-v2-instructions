After creating our schema, we need to do a few things:

1. Sync our DB and schema together
2. Generate a typesafe ORM based on our schema so we can interact with the DB

Luckily for us, prisma handles all of this for us. We can use the migrate command.

`npx prisma migrate dev`
