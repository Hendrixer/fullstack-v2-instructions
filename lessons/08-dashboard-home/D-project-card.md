## ProjectCard Component

Next, the project card. Create `/components/ProjectCard.tsx`

```javascript
import { FC } from "react";
import { Prisma } from "@prisma/client";
import Card from "./Card";
import clsx from "clsx";

const projectWithTasks = Prisma.validator<Prisma.ProjectArgs>()({
  include: { tasks: true },
});

type ProjectWithTasks = Prisma.ProjectGetPayload<typeof projectWithTasks>;

const formatDate = (date) =>
  new Date(date).toLocaleDateString("en-us", {
    weekday: "long",
    year: "numeric",
    month: "short",
    day: "numeric",
  });

const ProjectCard: FC<{ project: ProjectWithTasks }> = ({ project }) => {
  const completedCount = project.tasks.filter(
    (t) => t.status === "COMPLETED"
  ).length;
  const progress = Math.ceil((completedCount / project.tasks.length) * 100);

  return (
    <Card className="!px-6 !py-8 hover:scale-105 transition-all ease-in-out duration-200">
      <div>
        <span className="text-sm text-gray-300">
          {formatDate(project.createdAt)}
        </span>
      </div>
      <div className="mb-6">
        <span className="text-3xl text-gray-600">{project.name}</span>
      </div>
      <div className="mb-2">
        <span className="text-gray-400">
          {completedCount}/{project.tasks.length} completed
        </span>
      </div>
      <div>
        <div className="w-full h-2 bg-violet-200 rounded-full mb-2">
          <div
            className={clsx(
              "h-full text-center text-xs text-white bg-violet-600 rounded-full"
            )}
            style={{ width: `${progress}%` }}
          ></div>
        </div>
        <div className="text-right">
          <span className="text-sm text-gray-600 font-semibold">
            {progress}%
          </span>
        </div>
      </div>
    </Card>
  );
};

export default ProjectCard;
```

## Getting Project Data

We then need to get projects on the home page.

```ts
const getData = async () => {
  await delay(2000);
  const user = await getUserFromCookie(cookies());
  const projects = await db.project.findMany({
    where: {
      ownerId: user.id,
    },
    include: {
      tasks: true,
    },
  });

  return { projects };
};
```

We can now call `getData` in the page component and loop over the projects to create project cards.

```ts
async function Page() {
  const { projects } = await getData();

  //.....
  {
    projects.map((project) => (
      <div className="w-1/3 p-3" key={project.id}>
        <Link href={`/project/${project.id}`}>
          <ProjectCard project={project} />
        </Link>
      </div>
    ));
  }
}
```

## Fix The Seed Script

The original seed script never hashed the password so you won't be able to log in with the seeded user. To fix this, make sure your seed script hashes the password:

```javascript
// in prisma/seed.ts
create: {
// ...
  password: await hashPassword("password"),
// ...
}
```

## Delete Data and Reseed Database

The seed script uses an upsert which won't update the current user. Reset the database and reseed the data:

```bash
npx prisma migrate reset
```

> ✔️ Code Checkpoint: The current code for the application can be found on the [project-card branch](https://github.com/Hendrixer/fullstack-app-v2-app/tree/project-card).
