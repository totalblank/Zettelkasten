In the `app` directory, nested folders are normally mapped to URL paths. However, you can mark a folder as a Route Group to prevent the folder from being included in the route's URL path.

This allows you to organize your route segments and project files into logical groups without affecting the URL path structure.

A route group can be created by wrapping a folder's name in parenthesis: `(folderName)`.
## Organize routes without affecting the URL path

To organize routes without affecting the URL, create a group to keep related routes together. The folders in parenthesis will be omitted from the URL (e.g., `(marketing)` or `(shop)`).

![[Next.js - route groups.png]]

Even though routes inside `(marketing)` and `(shop)` share the same URL hierarchy, you can create a different layout for each group by adding a `layout.tsx` file inside their folders.

## Opting for loading skeletons on a specific route

To apply a loading skeleton via a `loading.tsx` file to a specific route, create a new route group (e.g., `/(overview)`), and then move your `loading.tsx` inside that route group.

![[Next.js - route groups - loading skeleton.png]]

Now, the `loading.tsx` file will only apply to your dashboard $\rightarrow$ overview
page instead of all your dashboard pages, without affecting the URL path structure.