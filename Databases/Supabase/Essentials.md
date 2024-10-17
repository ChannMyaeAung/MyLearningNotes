# Supabase Essentials



## Getting supabaseURL and supabaseKey for `createBrowserClient()`

- On our supabase project page, click on "Project settings" on the left panel and click on "API" under Configuration.

- Then, copy Project URL which we can use it as `"supabaseURL"` for `createBrowserClient()`.

- And then we need to store it in our **".env.local"** file.

  - ```.env
    NEXT_PUBLIC_SUPABASE_URL = // "Project URL"
    NEXT_PUBLIC_SUPABASE_ANON_KEY = // Anon key
    ```

  - For the ANON_KEY, we can copy anon key in our supabase "Project Settings" just below "Project URL".



`superbaseClient.ts`

```ts
import { createBrowserClient } from "@supabase/ssr";

export const supabaseBrowserClient = createBrowserClient(
  process.env.NEXT_PUBLIC_SUPABASE_URL!,
  process.env.NEXT_PUBLIC_SUPABASE_ANON_KEY as string
);
```

