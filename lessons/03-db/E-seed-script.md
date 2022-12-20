When building out our UI, we'll need data to actually bind to for our components to render.
We can mock this data out on many different levels. Hard code in the components, create a json file with fake data, don't use the ORM and use fake data, or we can seed the DB.
I prefer the later, this keeps us from going back and making changes to our code once we actually have real data and need to remove the mocks.

<br>

ALl we have to do is write a seed script. A seed script is just a peice of code that inserts fake data into our dev DB so we can use it for development.

<br>

Let's create a seed script. Make file in the prisma folder called `seed.ts`

```ts
import { hashPassword } from "@/lib/auth";
import { db } from "@/lib/db";
import { TASK_STATUS } from "@prisma/client";

const getRandomTaskStatus = () => {
  const statuses = [
    TASK_STATUS.COMPLETED,
    TASK_STATUS.NOT_STARTED,
    TASK_STATUS.STARTED,
  ];
  return statuses[Math.floor(Math.random() * statuses.length)];
};

async function main() {
  const user = await db.user.upsert({
    where: { email: "user@email.com" },
    update: {},
    create: {
      email: "user@email.com",
      firstName: "User",
      lastName: "Person",
      password: await hashPassword("password"),
      projects: {
        create: new Array(5).fill(1).map((_, i) => ({
          name: `Project ${i}`,
          due: new Date(2022, 11, 25),
        })),
      },
    },
    include: {
      projects: true,
    },
  });

  const tasks = await Promise.all(
    user.projects.map((project) =>
      db.task.createMany({
        data: new Array(10).fill(1).map((_, i) => {
          return {
            name: `Task ${i}`,
            ownerId: user.id,
            projectId: project.id,
            description: `Everything that describes Task ${i}`,
            status: getRandomTaskStatus(),
          };
        }),
      })
    )
  );

  console.log({ user, tasks });
}
main()
  .then(async () => {
    await db.$disconnect();
  })
  .catch(async (e) => {
    console.error(e);
    await db.$disconnect();
    process.exit(1);
  });
```

This will create a mock user with some projects and tasks that we can use to signup and test things.

<br>

Next, we have to tell prisma about this in our package.json. The seed script will be running using ts-node, so we need to create a new tsconfig for that run time. Create `tsconfig-seed.json` and add this. It's exactly the same as our main one with just a change on the module type.

```json
{
  "compilerOptions": {
    "target": "es5",
    "lib": ["dom", "dom.iterable", "esnext"],
    "allowJs": true,
    "skipLibCheck": true,
    "strict": false,
    "forceConsistentCasingInFileNames": true,
    "noEmit": true,
    "incremental": true,
    "esModuleInterop": true,
    "module": "CommonJS",
    "moduleResolution": "node",
    "resolveJsonModule": true,
    "isolatedModules": true,
    "jsx": "preserve",
    "baseUrl": ".",
    "plugins": [
      {
        "name": "next"
      }
    ],
    "paths": {
      "@/components/*": ["components/*"],
      "@/hooks/*": ["hooks/*"],
      "@/lib/*": ["lib/*"],
      "@/styles/*": ["styles/*"],
      "@/prisma/*": ["prisma/*"],
      "@/assets/*": ["assets/*"]
    }
  },
  "include": ["next-env.d.ts", ".next/types/**/*.ts", "**/*.ts", "**/*.tsx"],
  "exclude": ["node_modules"]
}
```

Next, lets adjust a package.json.

```json
{
  "prisma": {
    "seed": "ts-node -P tsconfig-seed.json -r tsconfig-paths/register --transpileOnly prisma/seed.ts"
  }
}
```
