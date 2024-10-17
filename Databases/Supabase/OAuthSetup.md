# OAuth Setup when setting up a supabase database



1. In supabase dashboard, go to "Authentication", then in CONFIGURATION sections, click on "Providers".

2. Choose any Auth Providers you like.

3. If we choose GitHub, we have to copy something called "Callback URL (for OAuth)" and in a new tab, go to your GitHub.

   - On your GitHub profile, click on "Setting", and then navigate to "Developer Setting" at the bottom.
   - Click on "New OAuth App", Give the application name, the URL of your development server.
   - Paste your copied "Callback URL (for OAuth)" to Authorization callback URL.
   - Click on "Register application".
   - After registering, the GitHub will generate a Client ID. Copy that ID and paste it in the Client ID section in supabase Authentication Providers.
   - Then go back to the GitHub site and click on "Generate a new client secret" and copy and paste it to the Client Secret Section of supabase Authentication Providers.
   - Click "Save" in supabase Authentication Providers and you're done.

4. For Google Auth, open a new tab, go to "Google Console Cloud".

   - On that, click on "New Project". Give the same project name. Then Click "CREATE".

   - On the left side panel, there will be something called "APIs & Services". Click on that and then click on "OAuth consent screen".

   - Choose appropriate User Type (e.g. External) then click on "CREATE".

   -  On the next page, type in the name of the app, select your "user support email".

   - At the bottom of the page, there will be "Developer contact information". Type in your email address.

   - And then click "SAVE AND CONTINUE".

   - PROCEED until "SUMMARY".

   - After creating it, we can see something called "Credentials" on our left panel.

   - Click on that "Credentials" and on the top bar, click on "+ CREATE CREDENTIALS" and then select "OAuth client ID".

   - Choose the application type.

   - At the bottom, for "Authorize redirect URLs", we have to go back to our supabase providers page and copy "Callback URL (for OAuth)" and then paste it in that "Authorize redirect URLs" to create OAuth client ID.

   - After that, we will be given, the Client ID and Client secret and we just need to copy them and paste them in our supabase providers and then enable sign in with Google.

   - Click "Save".

     