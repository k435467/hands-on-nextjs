# My notes

Practice [the nextjs tutorial](https://nextjs.org/learn).

- [My notes](#my-notes)
  - [Create a Next.js App](#create-a-nextjs-app)
  - [Navigate Between Pages](#navigate-between-pages)
    - [Link Component](#link-component)
  - [Assets, Metadata, and CSS](#assets-metadata-and-css)
    - [Assets and Images](#assets-and-images)
    - [Metadata and Head](#metadata-and-head)
    - [CSS Sytling](#css-sytling)
      - [Styled-JSX](#styled-jsx)
      - [CSS Module](#css-module)
      - [Global Styles](#global-styles)
      - [Styling Tips](#styling-tips)
  - [Pre-rendering and Data Fetching](#pre-rendering-and-data-fetching)
    - [Static Generation with Data using getStaticProps](#static-generation-with-data-using-getstaticprops)
    - [Fetching Data at Request Time with getServerSideProps](#fetching-data-at-request-time-with-getserversideprops)
    - [Client-side Rendering with SWR Hook](#client-side-rendering-with-swr-hook)
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

### Assets and Images

- `public/images` directory
- lazy loaded
- resizing & optimizing

```jsx
import Image from "next/image";

const YourComponent = () => (
  <Image
    priority // the image is preloaded with this attribute, otherwise it is lazy loaded.
    src="/images/profile.jpg" // Route of the image file
    height={144} // Desired size with correct aspect ratio
    width={144} // Desired size with correct aspect ratio
    alt="Your Name"
  />
);
```

### Metadata and Head

```jsx
import Head from "next/head";
```

```jsx
export default function FirstPost() {
  return (
    <>
      <Head>
        <title>First Post</title>
      </Head>
      <h1>First Post</h1>
    </>
  );
}
```

### CSS Sytling

#### Styled-JSX

`pages/index.js`

Write CSS directly in .js files.

```jsx
<style jsx>{`
  …
`}</style>
```

#### CSS Module

It scope styles at the component level.

- Import the CSS file and assign a name to it, like `styles`
- Use `styles.container` as the `className`

`components/layout.js`

```jsx
import styles from "./layout.module.css";

export default function Layout({ children }) {
  return <div className={styles.container}>{children}</div>;
}
```

For multiple components. `styles/utils.module.css`

**Important**: To use CSS Module, the CSS file name must end with `.module.cs`.

#### Global Styles

To load global CSS files, create a file called `pages/_app.js` with the following content:

```jsx
import "../styles/global.css";

export default function App({ Component, pageProps }) {
  return <Component {...pageProps} />;
}
```

**Important:** Need to restart the dev server when we add `pages/_app.js`.

#### Styling Tips

- Using **classnames** library to toggle classes
- Customizing **PostCSS** Config. **Wailwind CSS**.
- Using **Sass**

## Pre-rendering and Data Fetching

- **Static Generation** is the pre-rendering method that generates the HTML at **build time**. The pre-rendered HTML is then reused on each request.
- **Server-side Rendering** is the pre-rendering method that generates the HTML on **each request**.

### Static Generation with Data using getStaticProps

`getStaticProps` runs at build time in prodution, and we can fetch external data and send it as props to the page.

```jsx
export default function Home(props) { ... }

export async function getStaticProps() {
  // Get external data from the file system, API, DB, etc.
  const data = ...

  // The value of the `props` key will be
  //  passed to the `Home` component
  return {
    props: ...
  }
}
```

### Fetching Data at Request Time with getServerSideProps

```jsx
export async function getServerSideProps(context) {
  return {
    props: {
      // props for your component
    },
  };
}
```

Its parameter (**context**) contains request specific parameters.

### Client-side Rendering with SWR Hook

highly recommended.

- handles caching
- revalidation
- focus tracking
- refetching on interval

```jsx
import useSWR from "swr";

function Profile() {
  const { data, error } = useSWR("/api/user", fetch);

  if (error) return <div>failed to load</div>;
  if (!data) return <div>loading...</div>;
  return <div>hello {data.name}!</div>;
}
```

## Dynamic Routes

Staticlly generate pages with paths that depend on external data. This enables **dynamic URLs** in Next.js.

![image: How to Statically Generate Pages with Dynamic Routes](https://nextjs.org/static/images/learn/dynamic-routes/how-to-dynamic-routes.png)

## API Routes

Serverless functions. By creating a function inside the `pages/api` directory.

```jsx
// pages/api/hello.js

// // req = HTTP incoming message, res = HTTP server response
// export default function handler(req, res) {
//   // ...
// }

export default function handler(req, res) {
  res.status(200).json({ text: "Hello" });
}
```

- Do Not Fetch an API Route from **getStaticProps** or **getStaticPaths**
- **Preview Mode**: When writing a draft on a headless CMS and want to preview the draft immediately, and let Next.js to render these pages at request time instead of build time.
- Dynamic API Routes

## Deploying Your Next.js App

## TypeScript
