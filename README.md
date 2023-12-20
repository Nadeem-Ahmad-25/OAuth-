# **OAuth - cookie based authentication.**

- Login with google / Facebook. Also login with email.
- Replacing local storage or storing token in headers with cookies. Which leads to water falling problem of react going away
- When user comes to the website they can server side render their user details and course details etc.
That’s what gets rendered to the frontend.

- Initially what happened was the bundle came in and then the request goes out to fetch user details and then it gets re-rendered on the screen so this was the water falling problem of react.

# **Backend-**
-   The backend is responsible for implementing the authentication logic, handling OAuth flows, and managing user sessions and tokens securely.
-   OAuth authentication is typically performed on the backend server, where the server communicates with the OAuth provider (e.g., Google, GitHub) to verify the user's identity and obtain authorization tokens.
-   NextAuth, being a library designed for Next.js applications, is integrated into the backend of your application. It helps streamline the process of handling authentication on the server side.
# **Frontend-**

-   On the frontend, your React application needs to interact with the backend to initiate the authentication process and handle the response.
-   Once the user is authenticated on the backend, relevant user details and tokens are often sent to the frontend, allowing for a seamless user experience.
-   React components on the frontend can then use this information for rendering and potentially initiating further data fetches or rendering optimizations.

# **OAuth-**
What’s OAuth -

-   Your website registers with Google, gets back the credentials,
-   User comes to your website , you send them to google
-   Google calls back a redirect URL with a token
-   You do a server to server call to google to get back and access token
-   You use the access token gives you the access to the users email/photos/name etc.
- when you click on the ( sign in with google ) button it redirects you to [accounts.google.com](http://accounts.google.com) sign in page where a big URL is generated which contain bunch of parameters like -

-   ClientID - so the platform ([slides.com](http://slides.com) etc) it got it from google which can be publicly shared, as it is safe to use, because with this you can’t hack [slides.com](http://slides.com) or get their user details etc. secret ID is also shared which is not shared.
-   Domain of the website - [slides.com](http://slides.com) for us it will be localhost:3000
-   Scope - which means what access you want for the user. In our case email and profile picture. For example stream yard they will want access to your YouTube also along with it.
-   redirect_uri - which means if the person approves on google website pls callback this redirect_uri. which will redirect you to [slides.com](http://slides.com) along with a token. And then the google will know about the authencity of the user and let the [slides.com](http://slides.com) use your email your profile picture etc etc.
-   Basically a backend call is made from [slides.com](http://slides.com) to google with this token and then google will know this token came from this specific user and then sends back username, profile etc.
- So it’s bascially a 2 step process.

Step 1.

-   In the first callback you receive a token not the actual user details because if that would have been allowed anyone in the world could hit the auth endpoint and would get access to the data using your email id. So to protect this

Step 2.

-   [Slides.com](http://Slides.com) will sort of confirm from google with the token that are you the one who called this redirect_uri with this token. Google will approve if it’s the right token and then send your details to [slides.com](http://slides.com) to use.

Ok Few points to remember

nextAuth doesn’t work on all versions of NODE it doesn’t work specifically on node v18.1.0 it works on v16 and other higher version but not on v18.1.0 so make sure you upgrade it using “nvm install v20.5.1” and then “nvm use v20.5.1”

Starting with getting API Credentials for (google in our example here)
Logging in to [console.cloud.google.com/apis/credentials](http://console.cloud.google.com/apis/credentials) and start by making a new project and creating credentials by specifying Domain URL you have (in my case it was [www.dumbdotkom.com](http://www.dumbdotkom.com) and if you have business email from the DNS provider you can use that.
Once you create the client get the clientID and secret specified to give access. And use this link as redirect_uri = [http://localhost:3000/api/auth/callback/google](http://localhost:3000/api/auth/callback/google)

In the project you go too use this client ID in the controller [...nextauth].ts file

Using local.env in my case.

One can add GithubProvider using the similar approach or Facebook Provider etc etc.

Export the function is what you have to create  
under the hood NextAuth takes care of controllers which are nothing but get request post request etc. for different endpoints.

So the authOptions have two things
1.  Providers and 
2. Session
Session is created in index.ts file

Session is the state which store the status (authenticated or unauthenticated) and post login returns the data carrying the things we requested at the time of signup in our case email and profile.

So all of this happens in a similar way to as the UseContext hook in react

