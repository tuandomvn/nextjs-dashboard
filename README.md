## Next.js App


Study link next js

[https://nextjs.org/learn/dashboard-app](https://nextjs.org/learn/dashboard-app)
  

## Chapter 1: Getting Started

Server side render
"use client" --> Client component
By connecting your GitHub repository, whenever you push changes to your main branch, **Vercel** will automatically redeploy your application with no configuration needed

Prisma: automatically generates type script types based on your database schema.

  
## Chapter 2: CSS Styling

global CSS: global.css

Tailwind and CSS modules are the two most common ways of styling Next.js applications. Whether you use one or the other is a matter of preference - you can even use both in the same application!
 
[Tailwind](https://tailwindcss.com/) is a CSS framework that speeds up the development process by allowing you to quickly write [utility classes](https://tailwindcss.com/docs/utility-first) directly in your TSX markup.

[CSS Modules](https://nextjs.org/docs/basic-features/built-in-css-support) allow you to scope CSS to a component by automatically creating unique class names, so you don't have to worry about style collisions as well.

Using the clsx library to toggle class names

  
## Chapter 3 Optimizing Fonts and Images

**Fonts** can affect performance if the font files need to be fetched and loaded.

[Cumulative Layout Shift](https://web.dev/cls/) is a metric used by Google to evaluate the performance and user experience of a website

Next.js automatically optimizes fonts when you use the `next/font` module. It downloads font files at build time and hosts them with your other static assets so that there are no additional network requests.

Adding a primary font
Adding a secondary font
  
**Image Optimization**: use the `next/image` component to automatically optimize your images.
Use The `<Image>` Component
-   Ensure your image is responsive on different screen sizes.
   -   Specify image sizes for different devices.
   -   Prevent layout shift as the images load.
   -   Lazy load images that are outside the user's viewport.
    

## Chapter 4 Creating Layouts and Pages
this chapter to study creating more routes with layouts and pages.
**Nested routing**: Next.js uses file-system routing where folders are used to create nested routes.
Each folder represents a route segment that maps to a URL segment.
`app/page.tsx` --> home page associated with the route `/`
`app/page.tsx` is a special Next.js file that exports a React component
`/app/dashboard/page.tsx` is associated with the `/dashboard` path

![](https://www.evernote.com/shard/s570/res/9251f368-6901-dd87-b6ad-fdbd6fa78cf5)

 `/app/layout.tsx`: Root layout, is required, will be shared across all pages in your application
`/app/dashboard/layout.tsx` special file to create UI that is shared between multiple pages

If a file is in the parent folder (route), it will be applied for all sub-folders (route). For ex: loading.tsx

**[partial rendering](https://nextjs.org/docs/app/building-your-application/routing/linking-and-navigating#3-partial-rendering)**: on navigation, only the page components update while the layout won't re-render.

 ## Chapter 5: Navigating Between Pages

`<a>` HTML element --> full page refresh on each page navigation
`<Link />` Component --> link between pages in your application, [client-side navigation](https://nextjs.org/docs/app/building-your-application/routing/linking-and-navigating#how-routing-and-navigation-works) with JavaScript

**Automatic code-splitting and prefetching**
-   To improve the navigation experience, Next.js automatically code splits your application by route segments. This is different from a traditional React [SPA](https://developer.mozilla.org/en-US/docs/Glossary/SPA), where the browser loads all your application code on initial load.
    
Splitting code by routes means that pages become isolated. If a certain page throws an error, the rest of the application will still work.

-   Whenever `<Link>` components appear in the browser's viewport, Next.js automatically prefetches the code for the linked route in the background. By the time the user clicks the link, the code for the destination page will already be loaded in the background, makes the page transition near-instant!
    

## Chapter 6 Setting Up Your Database

Setting up a PostgreSQL database using `@vercel/postgres`

Set up Vercel account:

-   Connect and deploy your project
    
-   Connect to DB
    


## Chapter 7 Fetching Data

Use API layer: using [Route Handlers](https://nextjs.org/docs/app/building-your-application/routing/route-handlers).
-   when fetching data from the client
  -   when fetching data from API
   
Use Database queries

-   When need to write logic to interact to DB    
-   When use React Server Components (we can skip API layer)
    

  

**React Server Components** (execute on the server)
-   support promises, asynchronous tasks like data fetching. We You can use `async/await` syntax    
-   execute on the server, data fetches and logic on the server and only send the result to the client.    
-   No need an additional API layer.    

For example: export default async function Page() {}
-   Page is an async component. This allows you to use `await` to fetch data.

  

**Request waterfalls**

-   The data network requests are unintentionally blocking each other, creating a request **waterfall**. For example, you might want to fetch a user's ID and profile information first. Then load the friend list. This can unintentional and impact performance.
    

Parallel data fetching

-   By using `Promise.all()` or `Promise.allSettled()` --> improve performance
    

const data = await Promise.all([

invoiceCountPromiseSQL,

customerCountPromiseSQL,

invoiceStatusPromiseSQL,

]);

## Chapter 8 Static and Dynamic Rendering

**Dynamic Rendering (also Server side Rendering - SSR)**

**Static rendering**

With static rendering, data fetching and rendering happens on the server at build time (when you deploy) or during [revalidation](https://nextjs.org/docs/app/building-your-application/data-fetching/fetching-caching-and-revalidating#revalidating-data). The result can then be distributed and cached in a [Content Delivery Network (CDN)](https://nextjs.org/docs/app/building-your-application/rendering/server-components#static-rendering-default). Benefit:

-   Faster Websites - Prerendered content can be cached and globally distributed
    
-   Reduced Server Load - Because the content is cached
    
-   SEO - Prerendered content is easier for search engine crawlers to index, as the content is already available when the page loads. --> improved search engine rankings.
    

  

Static rendering is useful for UI with **no data or data that is shared across users**, such as a **static blog** post or a **product page**. It might not fit a regularly updated page

Automatic Static Optimization

Next.js automatically determines that a page is static (can be prerendered) if it has no blocking data requirements. This determination is made by the absence of `getServerSideProps` and `getInitialProps` in the page.

**Dynamic rendering**, content is rendered on the server for each user at the request time (when the user visits the page). There are a couple of benefits of dynamic rendering:

-   Real-Time Data    
-   User-Specific Content    
-   Request Time Information
    

Making the page dynamic
import { unstable_noStore as noStore } from 'next/cache';
Add noStore() to prevent the response from being cached.

  

Common challenge to solve:
With dynamic rendering, your application is only **as fast as your slowest data fetch**. (waiting for the slowest request)

![](https://www.evernote.com/shard/s570/res/3b9477ee-9fd6-1a74-e381-b8d8a5b60cdf)

  

## Chapter 9 Streaming

purpose: improve the user experience when there are slow data requests.
**Streaming:** data transfer technique that allows you to **break down a route into smaller** "chunks" and progressively stream them from the server to the client as they become ready
By streaming, you can prevent slow data requests from blocking your whole page. This allows the user to see and interact with parts of the page without waiting for all the data to load
Chunks are rendered in parallel, reducing the overall load time
Streaming works well with React's component model, as each component can be considered a _chunk_.

Implement:
-   At the page level, with the `loading.tsx` file.    
-   For specific components, with `<Suspense>`.
    

  

**Streaming a whole page** with loading.tsx ( loading div )

-   `loading.tsx` is a special Next.js file built on top of Suspense, it allows you to create fallback UI to show as a replacement while page content loads.    
-   Since `<SideNav>` is static, it's shown immediately. The user can interact with `<SideNav>` while the dynamic content is loading.    
-   The user doesn't have to wait for the page to finish loading before navigating away (this is called interruptable navigation).
    

  

Problem: Since `loading.tsx` is a level higher than `/invoices/page.tsx` and `/customers/page.tsx` in the file system, it's also applied to those pages --> fix by using **Logical groups**

**Logical groups:**

-   is a folder using parentheses `()`, the name won't be included in the URL path. So `/dashboard/(overview)/page.tsx` becomes `/dashboard`.    
-   Move `loading.tsx` and `page.tsx` to (overview). The organize files into logical groups will not affecting the URL path structure
    

**Streaming a component**

wrap dynamic components in Suspense --> defer rendering parts until data is loaded

  

**Summary**

-   Stream the whole page with `loading.tsx`... but that may lead to a longer loading time if one of the components has a slow data fetch.
    
-   Stream every component individually... but that may lead to UI _popping_ into the screen as it becomes ready.
    
-   Streaming page sections: create wrapper components.
    
  

## Chapter 10 Partial Prerendering (PPR)

**Partial Prerendering** – an experimental feature that allows you to render a route with a static loading shell, while keeping some parts dynamic. In other words, you can isolate the dynamic parts of a route

![](https://www.evernote.com/shard/s570/res/531e3b66-6b8c-c01d-7dd6-d83328828eae)

Partial Prerendering uses React's [Suspense](https://react.dev/reference/react/Suspense) to defer rendering parts

It's worth noting that wrapping a component in Suspense doesn't make the component itself dynamic (need to used `unstable_noStore` instead), but rather Suspense is used as a boundary between the static and dynamic parts of your route.

Enable PPR: by adding the `ppr` option to your `next.config.js`

  

The great thing about Partial Prerendering is that you don't need to change your code to use it. As long as you're using Suspense to wrap the dynamic parts of your route, Next.js will know which parts of your route are static and which are dynamic.

## Chapter 11 Adding Search and Pagination

**Searching**

Next.js client hooks:

`useSearchParams`: to access the parameters of the current URL
`usePathname`: read the current URL's pathname
`useRouter`: Enables navigation between routes within client components programmatically

URLSearchParams: a Web API that provides utility methods for manipulating the URL query parameters. Methods: set, delete URL params

`useRouter` has a function replace URL. The URL is updated without reloading the page --> client-side navigation
  

-   Client Component, so you used the `useSearchParams()` hook to access the params from the client (avoids having to go back to the server)    
-   Server Component, so you can pass the `searchParams` prop from the page to the component.
    

A client Component, you can use event listeners and hooks

**Pagination**: fetch data on server, and pass data to <Pagination/> client component

`usePathname` and `useSearchParams` hooks --> to get the current page and set the new page



# Chapter 12: Mutating Data using forms and React Server Actions.

**React Server Actions** allow you to run asynchronous code **directly** on the **server**. They eliminate the need to create API endpoints to mutate your data. Instead, you write asynchronous functions that execute on the server and can be invoked from your Client or Server Components.

They offer an effective **security solution**, protecting against different types of attacks, **securing** your data, and ensuring **authorized** access. Server Actions achieve this through techniques like POST requests, encrypted closures, strict input checks, error message hashing, and host restrictions, all working together to significantly enhance your app's safety.

use the `action` attribute in the `<form>` element to invoke action

Behind the scenes, Server Actions create a `POST` API endpoint. This is why you don't need to create API endpoints manually when using Server Actions.

Progressive Enhancement. This allows users to interact with the form and submit data even if the JavaScript for the form hasn't been loaded yet or if it fails to load.

By adding the `'use server'`, you mark all the exported functions within the file as server functions. These server functions can then be imported into Client and Server components, making them extremely versatile.

[Zod](https://zod.dev/), a TypeScript-first validation library

`revalidatePath` function

you're updating the data displayed in the invoices route, you want to clear this cache and trigger a new request to the server. You can do this with the `revalidatePath` function

Create a Dynamic Route Segment with the invoice id

![](https://www.evernote.com/shard/s570/res/d65c8dce-2b95-159e-220a-a761ab17a7e6)

Server Actions to mutate data

# Chapter 13 Handling Errors

error.tsx: catching all errors

`notFound` can be used when you try to fetch a resource that doesn't exist.

form validation. Let's see how to implement server-side validation with Server Actions, and how you can show form errors using the `useFormState` hook

# Chapter 14 Improving Accessibility

Accessibility refers to designing and implementing web applications that everyone can use, including those with disabilities. It's a vast topic that covers many areas, such as keyboard navigation, semantic HTML, images, colors, videos, etc.

By default, Next.js includes the `eslint-plugin-jsx-a11y` plugin to help catch accessibility issues early. For example, this plugin warns if you have images without `alt` text, use the `aria-*` and `role` attributes incorrectly, and more.

There are three things to improve accessibility:

**Semantic HTML**: This allows assistive technologies (AT) to focus on the input elements and provide appropriate contextual information to the user

**Labeling**: Including `<label>` and the `htmlFor` attribute ensures that each form field has a descriptive text label. This improves AT support

**Focus Outline:** The fields are properly styled to show an outline when they are in focus

_What are Semantic Elements? A semantic element clearly describes its meaning to both the browser and the developer._

_Examples of non-semantic elements: `<div>` and `<span>` - Tells nothing about its content._
_Examples of semantic elements: `<form>, <table>, and <article>` - Clearly defines its content._

**Client-Side validation**
-   using form validation provided by the browser by adding the `required` attribute    

**Server-Side validation**
-   Ensure your data is in the **expected format** before sending it to your database.    
-   Reduce the risk of malicious users by passing client-side validation.    
-   Have one source of truth for what is considered _valid_ data.
    

use Zod to validate form data: `use parse, safeParse()` function

[https://codedamn.com/news/javascript/zod-getting-started](https://codedamn.com/news/javascript/zod-getting-started)

  

`useFormState` is a hook, we will need to turn your form into a Client Component 'use client';

`useFormState` to call the server action and handle form errors, and display them to the user.

## Chapter 15: Authentication

  

using NextAuth.js to add authentication to your app.

Authentication: who they are

Authorization is the next step. authorization decides what parts of the application they are allowed to use.

**NextAuth.js** involved in managing sessions, sign-in and sign-out, and other aspects of authentication

Use **OpenSSL** to generate a secret key for your application. This key is used to encrypt cookies, ensuring the security of user sessions

C:\Program Files\Git\usr\bin\openssl.exe

OpenSSL command: openssl rand -base64 32

  

Create `authConfig` object contain the configuration options for NextAuth.js

`pages`: custom sign in, sign out, error pages

callbacks: is called before a request is completed, to verify if the request is authorized to access a page. It receives an object with the `auth` and `request` properties. The `auth` property contains the user's session, and the `request` property contains the incoming request.

  

`bcrypt` to hash the user's password

Provider: login options such as Google or GitHub, Credentials Provider, OAuth, email...

  

-   Email: [user@nextmail.com](mailto:user@nextmail.com)
    
-   Password: `123456`
    

## Chapter 16 Adding Metadata (SEO)

Metadata is crucial for SEO and shareability, it provides additional details about a webpage

Metadata is not visible to the users, it is embedded within the page's HTML, usually within the `<head>` element
enhancing a webpage's SEO
Types of metadata
-   Title Metadata: <title>Page Title</title>    
-   Description Metadata: <meta name="description" content="... content." />    
-   Keyword Metadata: <meta name="keywords" content="keyword1, keyword2, keyword3" />    
-   Open Graph Metadata: enhances the way a webpage is represented when shared on social media platforms    
`<meta property="og:title" content="Title Here" />`

`<meta property="og:description" content="Description Here" />`

`<meta property="og:image" content="image_url_here" />`

  

-   Favicon Metadata: <link rel="icon" href="path/to/favicon.ico" />
    

Adding metadata in NextJS
using Metadata API with 2 ways to add:
-   Config-based: `layout.js` or `page.js`, using an object or function    
-   File-based: using range of special files
    

`favicon.ico`, `apple-icon.jpg`, and `icon.jpg`: Utilized for favicons and icons
`opengraph-image.jpg` and `twitter-image.jpg`: Employed for social media images
`robots.txt`: Provides instructions for search engine crawling
`sitemap.xml`: website's structure

Next.js will automatically generate the relevant `<head>` element

We can define `metadata` object to include a template in layout.tsx, then apply in specific pages

  

