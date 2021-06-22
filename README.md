# My notes

Practice [the nextjs tutorial](https://nextjs.org/learn).

- [My notes](#my-notes)
  - [Create a Next.js App](#create-a-nextjs-app)
  - [Navigate Between Pages](#navigate-between-pages)
    - [Link Component](#link-component)
  - [Assets, Metadata, and CSS](#assets-metadata-and-css)
  - [Pre-rendering and Data Fetching](#pre-rendering-and-data-fetching)
  - [Dynamic Routes](#dynamic-routes)
  - [API Routes](#api-routes)
  - [Deploying Your Next.js App](#deploying-your-nextjs-app)
  - [TypeScript](#typescript)

```text
root
    components
    pages
        post
            first-post.js
        _app.js
        index.js
    public
        images
            profile.jpg
    styles
        global.css
        utils.module.css
    package-lock.json
    package.json
```

## Create a Next.js App

requirements:

- Node.js > 10.13

Create a Next.js app

```shell
npx create-next-app nextjs-blog --use-npm --example "https://github.com/vercel/next-learn-starter/tree/master/learn-starter"
```

Run the dev server

```shell
cd nextjs-blog
npm run dev
```

## Navigate Between Pages

Pages are associated with a route based on their file name. A page is a React Component exported from them.

- `pages/index.js` is associated with the `/` route.
- `pages/posts/first-post.js` is associated with the `/posts/first-post` route.

### Link Component

```jsx
import Link from "next/link";
```

```jsx
<h1 className="title">
  Read{" "}
  <Link href="/posts/first-post">
    <a>this page!</a>
  </Link>
</h1>
```

It is **client-side navigation**,which means that the page transition happens using JS . The browser does not do a full refresh.
Also see: **Code splitting and prefetching**.

**Note:** If we need to link to **external page** outside the Next.js app, just use an `<a>` tag without `<Link>`

**Note:** If we need to add **attributes**, add it to the `<a>` tag, not to the `<Link>` tag.

## Assets, Metadata, and CSS

## Pre-rendering and Data Fetching

## Dynamic Routes

## API Routes

## Deploying Your Next.js App

## TypeScript
