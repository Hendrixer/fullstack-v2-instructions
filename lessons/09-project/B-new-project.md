To create a new project, we need to make a component that shows the UI and makes an API request. Let's the make API function in the API lib

```ts
export const createNewProject = (name) => {
  return fetcher({
    url: "/api/project",
    method: "POST",
    body: { name },
  });
};
```

Then the API handler in `page/api/project`

```ts
import { validateJWT } from "@/lib/auth";
import { db } from "@/lib/db";

export default async function handler(req, res) {
  const user = await validateJWT(req.cookies[process.env.COOKIE_NAME]);

  await db.project.create({
    data: {
      name: req.body.name,
      ownerId: user.id,
    },
  });

  res.json({ data: { message: "ok" } });
}
```

And finally, a client component

```ts
"use client";
import { createNewProject } from "@/lib/api";
import { useState } from "react";
import Modal from "react-modal";
import Button from "./Button";
import Input from "./Input";

Modal.setAppElement("#modal");

const NewProject = () => {
  const [modalIsOpen, setIsOpen] = useState(false);
  const openModal = () => setIsOpen(true);
  const closeModal = () => setIsOpen(false);
  const [name, setName] = useState("");

  const handleSubmit = async (e) => {
    e.preventDefault();
    await createNewProject(name);
    closeModal();
  };

  return (
    <div className="px-6 py-8 hover:scale-105 transition-all ease-in-out duration-200 flex justify-center items-center">
      <Button onClick={() => openModal()}>+ New Project</Button>

      <Modal
        isOpen={modalIsOpen}
        onRequestClose={closeModal}
        overlayClassName="bg-[rgba(0,0,0,.4)] flex justify-center items-center absolute top-0 left-0 h-screen w-screen"
        className="w-3/4 bg-white rounded-xl p-8"
      >
        <h1 className="text-3xl mb-6">New Project</h1>
        <form className="flex items-center" onSubmit={handleSubmit}>
          <Input
            placeholder="project name"
            value={name}
            onChange={(e) => setName(e.target.value)}
          />
          <Button type="submit">Create</Button>
        </form>
      </Modal>
    </div>
  );
};

export default NewProject;
```

Add this component to the dashboard home
