## Route Groups

The routes for app will be as follows:

```bash
/register
/signin
/home
/project/[id]
```

The home and project routes share the same layout, both being part of the dashboard. Signin and register share the same layout with each other as well. Because all 4 routes are on the same parent segment `/`, they all would share the same layout.

We don't want this. So we'll use route grouping here. This will allow us to have two root layouts without adding segments to the url.

```bash
(auth)
  layout
  register
    page
  signin
    page
(dashboard)
  layout
  home
    page
  project
    [id]
      page
```

> ✔️ Code Checkpoint: The current code for the application can be found on the [routes branch](https://github.com/Hendrixer/fullstack-app-v2-app/tree/routes).
