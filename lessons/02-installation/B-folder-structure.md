## app

All of our pages will go here. Using the app directory instead of the pages directory means we're opting into the server / client components architecture.

## assets

Nice place to put any images we might have.

## components

Any components that aren't pages.

## lib

Non-component, utility code used all over the app. Things like hashing passwords, checking JWT tokens, fetching data from the API, etc.

## pages

Our API routes will live in `/pages/api`. API have yet to be migrated from the pages directory.

> Note: API Routes have been migrated in Next.js 13.2 however this course uses Next.js 13.1. More information about the change [can be found here](https://nextjs.org/blog/next-13-2#custom-route-handlers)

## styles

All our styles
