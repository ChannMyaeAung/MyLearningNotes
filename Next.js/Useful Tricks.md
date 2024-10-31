# "Useful Tricks" worth reviewing

## `pathname.split("/").pop()` to extract the last part of the URL path.

### Code Explanation:

```jsx
"use client";

import { usePathname } from "next/navigation";

import React from "react";

export default function Navbar() {

 const pathname: string = usePathname();

 return (

    <div>

      <div>{pathname.split("/").pop()}</div>

  </div>

 );

}
```



### Step-by-Step Breakdown:

1. **Get the Pathname**:

   ```jsx
   const pathname: string = usePathname();
   ```

   The `pathname` variable holds the current pathname of the URL. For example, if the URL is `http://localhost:3000/dashboard/users`, `pathname` will be `/dashboard/users`.

2. **Split the Pathname**:

   ```jsx
   pathname.split("/")
   ```

   The `split("/")` method splits the pathname string into an array of substrings, using the `/` character as the delimiter. For the pathname `/dashboard/users`, the result will be:

   ```js
   ["", "dashboard", "users"]
   ```

3. **Get the Last Element**:

   ```jsx
   pathname.split("/").pop()
   ```

   The `pop()` method removes and returns the last element of the array. In this case, it will return `"users"`.

### Example:

Let's see how it works with different pathnames:

- **Pathname**: `/dashboard/users`
  - **Split Result**: `["", "dashboard", "users"]`
  - **Pop Result**: `"users"`
- **Pathname**: `/dashboard/settings`
  - **Split Result**: `["", "dashboard", "settings"]`
  - **Pop Result**: `"settings"`
- **Pathname**: `/`
  - **Split Result**: `["", ""]`
  - **Pop Result**: `""` (empty string)

### Why It Works:

- The [`split("/")`](vscode-file://vscode-app/c:/Users/User/AppData/Local/Programs/Microsoft VS Code/resources/app/out/vs/code/electron-sandbox/workbench/workbench.esm.html) method breaks the pathname into parts based on the `/` character.
- The [`pop()`](vscode-file://vscode-app/c:/Users/User/AppData/Local/Programs/Microsoft VS Code/resources/app/out/vs/code/electron-sandbox/workbench/workbench.esm.html) method retrieves the last part of the split array, which is the last segment of the pathname.

### Conclusion:

The code `pathname.split("/").pop()` effectively extracts the last segment of the URL path by splitting the pathname into an array and then retrieving the last element of that array. This is why, for the pathname `/dashboard/users`, it returns `"users"`.



## `/dashboard/users/add` to `/dashboard/users`

To obtain the previous path from the current pathname (e.g., `/dashboard/users/add` to `/dashboard/users`), we can split the pathname string by `'/'`, remove the last segment, and then join the remaining parts back together.

### Explanation:

- **Split the Pathname**:

  ```jsx
  const pathSegments = pathname.split("/");
  ```

  - This converts `/dashboard/users/add` into `["", "dashboard", "users", "add"]`.

- **Remove the Last Segment**:

  ```jsx
  const previousPath = pathSegments.slice(0, -1).join("/") || "/";
  ```

  - `slice(0, -1)` removes the last element (`"add"`), resulting in `["", "dashboard", "users"]`.

  - `join("/")` joins the array back into a string: `"/dashboard/users"`.
  - The `|| "/"` ensures that if the result is an empty string (which can happen if the pathname is `"/"`), it defaults to `"/"`.

- **Ensure Leading Slash**:

  ```jsx
  const adjustedPreviousPath = previousPath.startsWith("/") ? previousPath : `/${previousPath}`;
  ```

  - This ensures that the path starts with a `/`, which is important for URL paths.

### Usage:

- **Navigating Back**: We can use the `adjustedPreviousPath` variable to navigate back to the previous page or display it as needed.

### Example Scenarios:

1. **From `/dashboard/users/add`**:
   - Splits to `["", "dashboard", "users", "add"]`.
   - Removes `"add"`, joins to `"/dashboard/users"`.
2. **From `/dashboard`**:
   - Splits to `["", "dashboard"]`.
   - Removes `"dashboard"`, joins to `""`.
   - Defaults to `"/"` after applying `|| "/"`.
3. **From `/` (root path)**:
   - Splits to `["", ""]`.
   - Removes the last empty string, joins to `""`.
   - Defaults to `"/"`.

### Summary:

By splitting the pathname and removing the last segment, we can navigate back to the previous path in a URL hierarchy. This method is straightforward and works well within Next.js applications.



## Use query value to search for things instead of `useState()` and `useEffect()` in next.js

**Search.tsx**

```tsx
"use client";
import { usePathname, useRouter, useSearchParams } from "next/navigation";
import React from "react";
import { MdSearch } from "react-icons/md";

export default function Search({ placeholder }: { placeholder: string }) {
  const searchParams = useSearchParams();
  const { replace } = useRouter();
  const pathname = usePathname();

  const handleSearch = (e: React.ChangeEvent<HTMLInputElement>) => {
    const params = new URLSearchParams(searchParams);

    if (e.target.value) {
      params.set("q", e.target.value);
    } else {
      params.delete("q");
    }
    replace(`${pathname}?${params}`);
  };

  return (
    <div className="flex items-center gap-2 p-2 bg-transparent rounded-lg w-max">
      <MdSearch size={20} />
      <input type="text" placeholder={placeholder} onChange={handleSearch} />
    </div>
  );
}

```

```
# it will update as we type in a search bar
http://localhost:3000/dashboard/users?q=Chan
```

