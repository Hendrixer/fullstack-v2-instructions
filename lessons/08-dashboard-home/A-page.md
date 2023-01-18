## Home Page

Let's create the dashboard home page

```ts
import { delay } from "@/lib/async";
import { getUserFromCookie } from "@/lib/auth";
import { db } from "@/lib/db";
import { cookies } from "next/headers";
import Link from "next/link";
import { Suspense } from "react";

export default async function Page() {
  return (
    <div className="h-full overflow-y-auto pr-6 w-1/1">
      <div className=" h-full  items-stretch justify-center min-h-[content]">
        <div className="flex-1 grow flex">{/** greetings here */}</div>
        <div className="flex flex-2 grow items-center flex-wrap mt-3 -m-3 ">
          {/** projects map here */}
          <div className="w-1/3 p-3">{/* new project here */}</div>
        </div>
        <div className="mt-6 flex-2 grow w-full flex">
          <div className="w-full">{/* tasks here */}</div>
        </div>
      </div>
    </div>
  );
}
```

## Delaying API Calls

This `delay` method will be used to slow down API calls by delaying network requests. Copy this code into a new file `/lib/async.ts`.

```javascript
export const delay = (time) =>
  new Promise((resolve) => {
    setTimeout(() => resolve(1), time);
  });
```
