Next.js is a React framework that provides the building blocks to create web applications.

**Framework:** Next.js handles the tooling and configuration needed for  React, and provides additional structure, features, and optimizations for the application.

To build a complete web application with React from scratch, there are many important details that needs to be considered,

* Code has to be bundled using a bundler like webpack and transformed using a compiler like Babel.
* Need to do production optimizations such as code splitting.
* You might want to statically pre-render some pages for performance and SEO. You might also want to use server-side rendering or client-side rendering.
* You might have to write some server-side code to connect your React app to your data store.

## Project Structure

* `/app`: Contains all the routes, components, and logic for the application, this is where you'll be mostly working from.
* `/app/lib`: Contains functions used in the application, such as reusable utility functions and data fetching functions.
* `/app/ui`: Contains all the UI components for the application, such as cards, tables, and forms.
* `/public`: Contains all the static assets for the application, such as images.
* Config Files: Later.

## Styling

* Global styles: Place a file `global.css` inside of the `/app/ui` folder. Import this file at the root layout -- `import '@/app/ui/global.css';`
* Tailwind
* CSS Modules allow you to scope CSS to a component by automatically creating unique class names, so you don't have to worry about style collisions. It can be used like so -- `import styles from '@/app/ui/home.module.css';`
* `clsx` library can be used to toggle class names.
## Fonts and Images

Use `next/font` and `next/image`.

**Fonts:** Next.js automatically optimizes fonts in the application when you use the `next/font` module. It downloads font files at build time and hosts them with your other static assets. This means when a users visits your application, there are no additional network requests for fonts which would impact performance. Fonts can be optimized and 

```TypeScript
/* /app/ui/fonts.ts */

import { Inter, Lusitana } from 'next/font/google'; 

export const inter = Inter({ subsets: ['latin'] });

export const lusitana = Lusitana(
	{
		weight: ['400', '700'],
		subsets: ['latin'],
	}
);
```

**Images:** Images can be optimized by using the `next/image` component. The `<Image>` Component is an extension of the HTML `<img>` tag, and comes with automatic image optimizations, such as:

* Preventing layout shift automatically when images are loading.
* Resizing images to avoid shipping large images to devices with a smaller viewport.
* Lazy loading images by default (images load as the enter the viewport).
* Serving images in modern formats, like WebP and AVIF, when the browser supports it.

## Layouts and Pages

Next.js uses file-system routing where **folders** are used to create nested routes. Each folder represents a **route segment** that maps to a **URL segment**.

![[Next.js - nested routing.png]]

You can create separate UIs for each route using `layout.tsx` and `page.tsx` files.

* `page.tsx` is a special Next.js file that exports a React component, and it is required for the route to be accessible. So you can create different pages in Next.js by creating a new route segment using a folder, and add a `page.tsx` file inside it.
* `layout.tsx` is a special Next.js file that exports a React component (a layout). A layout is a UI shared between multiple pages. On navigation, layouts preserve state, remain interactive, and do not re-render. A layout can be defined by exporting a React component from a `layout.tsx` file. The component should accept a `children` prop which can be a page or another `layout`.

The benefit of using layouts in Next.js is that on navigation, only the page components update while the layout won't re-render.

![[Next.js - layout.png]]

## `<Link>` Component

* Prevents a full refresh.
* Imported from `next/link`

Although parts of the application is rendered on server, there is no full refresh. This is because of [Automatic code-splitting and Prefetching](https://nextjs.org/learn/dashboard-app/navigating-between-pages#automatic-code-splitting-and-prefetching). 

## Fetching Data

Different approaches to fetch data are,
* APIs
* ORMs
* SQL

Object-Relational Mapping (ORM) in computer science is a programming technique for converting data between a relational database and the memory of an object-oriented programming language. This creates, in effect, a virtual object database that can be used from within the programming language. ORM generates SQL under the hood.
### API Layer

APIs are an intermediary layer between the application code and database. There are a few cases where you might use an API,

* If you're **using third-party services** that provide an API
* If you're **fetching data from the client**, you want to have an API layer that runs on the server to avoid exposing your **database secrets** to the client. In this case, API works as an intermediary between the database and the client.

There are a few cases where you have to write database queries,

* When creating your API endpoints, you need to write logic to interact with your database.
* If you are using [[React Server Components]] (fetching data on the server), you can skip the API layer, and query your database directly without risking exposing your database secrets to the client.
### Using Server Components to fetch data

Next.js by default use [[React Server Components]]. Benefits of fetching data using Server Components are,

* Server Components support [[JavaScript Promises]], providing a solution for asynchronous tasks like data fetching natively. You can use `async/await` syntax without needing `useEffect`, `useState` or other data fetching libraries.
* Server Components run on the server, so you can keep expensive data fetches and logic on the server, only sending the result to the client.
* Since Server Components run on the server, you can query the database directly without an additional API layer. This saves you from writing and maintaining additional code.

SQL can be executed using [`postgres.js`](https://github.com/porsager/postgres)

When we need to display sorted data, we must retrieve sorted data using SQL query. Retrieving all the data and then sorting it (in the server) is a bad idea because it consumes too much memory.

## Request Waterfalls

A "waterfall" refers to a sequence of network requests that depend on the completion of previous requests. In the case of data fetching, each request can only begin once the previous request has returned data.

![[Next.js - waterfall data fetching.png]]

For example, in the following code,

```TypeScript
  const revenue = await fetchRevenue();
  const latestInvoices = await fetchLatestInvoices(); // wait for fetchRevenue() to finish
  const {
    numberOfCustomers,
    numberOfInvoices,
    totalPaidInvoices,
    totalPendingInvoices
  } = await fetchCardData(); // wait for fetchLatestInvoices() to finish

```

This method of data fetching is good and bad,

* **Good** - There may be cases where you want waterfalls because you want a condition to be satisfied before you make the next request. For example, you might want to fetch a user's ID and profile information first. Once you have the ID, you might then proceed to fetch their list of friends. In this case, each request is contingent on the data returned from the previous request.
* **Bad** - Waterfalls can also be unintentional and impact performance. A common way to avoid waterfalls is to initiate all data requests at the same time - in parallel. 

## Parallel Data Fetching

`Promise.all()` and `Promise.allSettled()` can be used to initiate all promises at the same time. 

## Static and Dynamic Rendering

With static rendering, data fetching and rendering happens on the server at build time (when you deploy) or when revalidating data. Whenever a user visits your application, the cached data is served. Benefits of static rendering are,
* **Faster Websites** - Pre-rendered content can be cached and globally distributed when deployed to platforms like Vercel.
* Reduced Server Load - Because the content is cached, your server does not have to dynamically generate content for each user request which reduces compute costs.
* SEO - Pre-rendered content is easier for search engine crawlers to index, as the content is already available when the page loads. This can lead to improved search engine rankings.
Static rendering is useful for UI with no data or data that is shared across users, such as a static blog or a product page. It might not be a good fit for a dashboard that has personalized data which is regularly updated.

With dynamic rendering, content is rendered on the server for each user at request time (when the user visits the page). Benefits of dynamic rendering are,
* **Real-Time Data** - Dynamic rendering allows your application to display real-time or frequently updated data. 
* **User-Specified Content**
* **Request Time Information** - Dynamic rendering allows you to access information that can only be known at request time, such as cookies or the URL search parameters.
With dynamic rendering, **your application is only as fast as your slowest data fetch.** But there are ways to improve the user experience where there are slow data requests - [Streaming](#streaming).

## Streaming

Streaming is a data transfer technique that allows you to break down a route into smaller "chunks" and progressively stream them from the server to the client as they become ready.

![[Next.js - streaming.png]]

By streaming, you can prevent slow data requests from blocking your whole page. This allows the user to see and interact with parts of the page without waiting for all the data to load before any UI can be shown to the user.

![[Next.js - streaming 2.png]]

Streaming works well with React's component model, as each component can be considered a *chunk*. There are two ways to implement streaming in Next.js:
1. At the page level, with the `loading.tsx` file (which creates `<Suspense>` for you).
2. At the component level, with `<Suspense>` for more granular control.

A loading skeleton is a simplified version of the UI. Many websites use them as a placeholder (or fallback) to indicate to users that the content is loading.

For example, let's say inside of a route (let's say `app/dashboard/`), there are `layout.tsx`, `loading.tsx` and `page.tsx`.

* `page.tsx` is what is rendered when the user requests this path.
* `layout.tsx` is the code that is shared among all the sub-routes of this path.
* `loading.tsx` is a special Next.js file built on top of React suspense. It allows you to create fallback UI to show as a replacement while page content loads. All the static parts of the `layout.tsx` are shown immediately.
Check out [[Route Groups]] for more info. Refer to the [documentation](https://nextjs.org/learn/dashboard-app/streaming) for more info on streaming.

### Streaming a component

Specific components can be streamed using React `Suspense`.

Suspense allows you to defer rendering parts of your application until some condition is met (e.g., data is loaded). You can wrap your dynamic components in Suspense. Then, pass it a fallback component to show while the dynamic component loads. For example,

```TypeScript
<Suspense fallback={<RevenueChartSkeleton />}>
  <RevenueChart />
</Suspense>
```

In the above example, a fallback component `<RevenueChartSkeleton />` is provided and the  main component to be rendered is `<RevenueChart />`. If the main component to be rendered has any operation can be a bottleneck, the fallback component is rendered and displayed. When the main component is ready, it is rendered and the fallback component is withdrawn.

### Grouping components

Let's say you need to import multiple components at the same time. Create a new wrapper component which groups all the components to show simultaneously and wrap the wrapper component in a `Suspense`.

For example,

```TypeScript
<div className="grid gap-6 sm:grid-cols-2 lg:grid-cols-4">
<Suspense fallback={<CardSkeleton />}>
  <CardWrapper />
</Suspense>
```

The above code groups the following code,

```TypeScript
export default async function CardWrapper() {
  const {
    numberOfCustomers,
    numberOfInvoices,
    totalPaidInvoices,
    totalPendingInvoices
  } = await fetchCardData(); // wait for fetchLatestInvoices() to finish
  return (
    <>
      <Card title="Collected" value={totalPaidInvoices} type="collected" />
      <Card title="Pending" value={totalPendingInvoices} type="pending" />
      <Card title="Total Invoices" value={numberOfInvoices} type="invoices" />
      <Card
        title="Total Customers"
        value={numberOfCustomers}
        type="customers"
      />
    </>
  );
}
```

### Where to place your `Suspense` boundaries

Depends on a few things,
1. How you want the user to experience the page as it streams.
2. What content you want to prioritize.
3. If the components rely on data fetching.

There are a few options,

* You could stream the **whole page** with a `loading.tsx`, but that may lead to a longer loading time if one of the components has a slow data fetch.
* You could stream **every component** individually, but that may lead to UI popping into the screen as it becomes ready.
* You could also create a staggered effect by streaming **page sections**. But you'll need to create wrapper components.

Generally, it's good practice to move the component wise data fetches down to the components that need it, and then wrap those components in `Suspense`.

### Search and Pagination

The search functionality will span the client and the server. When a user searches for an invoice on the client,

* The URL params will be updated
* Data will be fetched on the server
* And the table will re-render on the server with the new data

There are a couple of benefits of implementing search with URL params,

* Bookmarkable and sharable URLs: Since the search parameters are in the URL, users can bookmark the current state of the application, including their search queries and filters, for future reference or sharing.
* Server-side rendering: URL parameters can be directly consumed on the server to render the initial state, making it easier to handle server rendering.
* Analytics and tracking: Having search queries and filters directly in the URL makes it easier to track user behavior without requiring additional client-side logic.

Following are the Next.js client hooks that will be used to implement the search functionality:

* `useSearchParams` - allows you to access the parameters of the current URL. For example, the search params for this URL `/dashboard/invoices?page=1&query=pending` would look like this: `{page: '1', query: 'pending'}`.
* `usePathname` - lets you read the current URL's pathname. For example, for the route `/dashboard/invoices`, `usePathname` would return `/dashboard/invoices`.
* `useRouter` - enables navigation between routes within client components programmatically.

Implementation steps:
1. **Capture the user's input** - can be easily done by connecting an event listener function to the input field.
2. **Update the URL with the search params** - calling the Next.js hook [[useSearchParams | 'useSearchParams`]] inside of the component gives you the search parameters. As this is a react hook, it captures dynamic data from the 
3. Keep the URL in sync with the input field
4. Update the table to reflect the search query



One good option is to deploy in Vercel. Push the code in GitHub and deploy the whole project using Vercel. 
# Concepts

There are a few things to consider when building modern applications. Such as:

* **User Interface:** how users will consume and interact with the application.
* **Routing:** How users navigate between different parts of the application.
* **Data fetching:** where the data lives and how to get it.
* **Rendering:** when and where to render static or dynamic content.
* **Integrations:** what third-party services to use (for CMS, auth, payments, etc.) and how to connect them.
* **Infrastructure:** where to deploy, store, and run the application code (serverless, CDN, edge, etc.)
* **Performance:** how to optimize the application for end-users.
* **Scalability:** how the application adapts as the team, data, and traffic grow.
* **Developer Experience:** the team's experience building and maintaining the application.

# Tips

Git ignores `lib` folder by default. Make sure to add `!lib/` in `.gitignore`.

