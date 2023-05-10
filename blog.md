# How to create a React app with a Firebase Project ⚛️ 🔥

This post is a walkthrough of how to configure and develop a React application written in Typescript with a Firebase backend service.
It will touch some basic setup of Firebase for a React App with an example registration login and authorization feature.
There is a link to the repository in the TLDR section. It is meant as a setup for a meetup on how to use React and Firebase in an example app. It 's basically an encouragement to create your own apps using React and Firebase or even explore other frameworks besides React(please note that other frameworks will not be mentioned here). I hope you will have fun building this and later on your own apps using Firebase in the future.

What we will need:

- Node version of at least [lts](https://en.wikipedia.org/wiki/Long-term_support#:~:text=September%202022) which is currently is v16.18.0
- Your favorite text editor(I will be using [vscode](https://code.visualstudio.com/))
- A Firebase account we will set this up later so relax and don't worry about it for now.
- Also make sure [Java](https://www.java.com/en/download/help/download_options.html) is installed on your machine we need it to make the firebase emulator work.
- And the latest version of [Vite](https://vitejs.dev/) which we will need to create a React project, we will also go through the proces off installing this later on.

What Basic knowlegde this blog expects you to have:

- Some basic commandline knowledge
  - Creating directories
  - Changing directories
  - Running script commands
  - That sort of stuff
- Some Basic React and JS/TS knowledge is welcome but we will cover some of the basics and give some explainations so little knowledge is fine to. just as long as you know what a frontend framework is and have some javascript knowledge you will be fine.
  - We will be using TypeScript but not explain the typing so some knowledge is helpfull.
- Also some basic knowledge of Git will come in handy because some concepts will be mentioned but not explained.

Don't worry, this blog will explain how to set up Vite and create a Firebase project. Along the way, more dependencies will be added and also explained why we need it.

## TLDR

For those of you who just want to checkout the code:
The project can be found [here](https://github.com/kevinegstorf/firebase_basics_with_react).
And if you want to go step by step of creating the project just stick around and keep reading.

So lets get cracking!! 🐙

## Setting up Firebase

Go to this [link]('https://firebase.google.com/') and register for a Firebase account.
When you are done with the registration go to the firebase console and click on the _add project tile_ and you will go through some steps on how to create a project.
In the first step you have to give your project any name you like (*please note that a project name is not the same as an app name, later in the process you will add a name for your app.) Next you will be prompted if you would like to use google analytics, we will not discuss the google analytics bit in this blog but you are ofcourse free to select it and explore the feature by yourself, firebase recomments it is selected so i guess that is fine. in the next step just select the default account for firebase option and press create project.
When you are done creating the project you are now redirected to you project console overview page of the project.

For now we are done with Firebase and will continue after setting up a React project in the __Configure Firebase in the React Project__ part of this Blog.

## Creating a Fresh React project with Vite

This blog will not take a deepdive into Vite we will just touch on how to create a boilerplate React project which will save us a ton of time that we can actually use to create the good stuff.
Just a bit of info on Vite as you can probably guess by now creates boilerplate projects and is able to do that for most popular frontend frameworks its pretty cool to say the least. If you want to know a bit more about it go check out the Vite webpage.

First lets install vite globally so we can easily create boilerplate projects.
To install Vite just run this command in the terminal: `npm i -g vite`
the `-g` flag will make sure its installed globally.

Next find the folder where you would like to store the project and run the next command but with the name of the project of you own choice, I have chosen _firebase_basics_with_react_ but remember to replace the text in the command with your own project name or else you will have a project with the same name.

`npm init vite@latest firebase_basics_with_react -- --template react-ts`

After this is done vite will promt in the CLI something like this

```bash
Done. Now run:

  cd firebase_basics_with_react
  npm install
  npm run dev
```

Lets just copy this and past in the commandline.

Now you can visit `http://localhost:5173/` in the browser and your Vite React project in Typescript should be all set.
It is time to use your favorite text editor and start coding!

## Clean up the React project

Now open up the project in your text editor.
Lets get rid off some of the boilerplate and make the App more like it is our own.
go to the the root folder of the project and open the `src` folder.

It should look a little like this.

```bash
src
> assets
App.css
App.tsx
index.css
main.tsx
vite-env.d.ts
```

We will just delete all content in the `App.tsx` file and leave the rest for now, everything will be updated in time.
create a `firebase` folder inside the `src` folder and give in an `index.ts` file.

## Configure Firebase in the React Project

Now that you have a Firebase Project and a fresh new React boilerplate project, lets combine the two and start our app building adventure.
Go back to the firebase console of your brand new project.

Now select the button with closing brackets `</>`, this will take us to the place where we can create a web application.

Now you can Register your app and think of a name for it. Leave the hosting checkbox empty for now we will talk about it later.
and hit the register app button.

The next step is to add the firebase SDK to our project that is where the index.ts file in the firebase folder is for that we created earlier.
We will be using NPM so we can follow the steps that are presented in the firebase webpage.
so go over to the commandline the root folder of your react app and  run the npm install command.

and run:
`npm install firebase`

next up create a file called `.env` in the root of you project.
and create these environment variables.

```txt
VITE_API_KEY=
VITE_AUTH_DOMAIN=
VITE_PROJECT_ID=
VITE_STORAGE_BUCKET=
VITE_MESSAGE_SENDER_ID=
VITE_APP_ID=
VITE_MEASUREMENT_ID=
```

When you look In the firebase console in the browser underneath the `npm install firebase` script, there is an example file of how to initialize the firebase app inside the react app.

It looks something like this:

```javascript
// Import the functions you need from the SDKs you need
import { initializeApp } from "firebase/app";
import { getAnalytics } from "firebase/analytics";
// TODO: Add SDKs for Firebase products that you want to use
// https://firebase.google.com/docs/web/setup#available-libraries

// Your web app's Firebase configuration
// For Firebase JS SDK v7.20.0 and later, measurementId is optional
const firebaseConfig = {
  apiKey: "YOUR KEY",
  authDomain: "YOUR DOMAIN",
  projectId: "YOUR PROJECT ID",
  storageBucket: "YOUR STORAGE BUCKET",
  messagingSenderId: "YOUR MESSAGING SENDER ID",
  appId: "YOUR APP ID",
  measurementId: "YOUR MEASUREMENT ID"
};

// Initialize Firebase
const app = initializeApp(firebaseConfig);
const analytics = getAnalytics(app);
```

The values in the firebaseConfig object can be pasted in the corresponding variables of the `.env` file.
Please note that the `.env` file is added to the `.gitignore` file of your project, if you are planning to use a git repository for your project. __Never push keys to an open source repo!__, _actually_ __Never push keys to any repo for security sake!__.

now lets paste the initialize file inside the project. It needs to be inside the index.ts file of the firebase folder.

```typescript
import { initializeApp } from 'firebase/app';

// Your web app's Firebase configuration
// For Firebase JS SDK v7.20.0 and later, measurementId is optional
const firebaseConfig = {
  apiKey: import.meta.env.VITE_API_KEY,
  authDomain: import.meta.env.VITE_AUTH_DOMAIN,
  projectId: import.meta.env.VITE_PROJECT_ID,
  storageBucket: import.meta.env.VITE_STORAGE_BUCKET,
  messagingSenderId: import.meta.env.VITE_MESSAGE_SENDER_ID,
  appId: import.meta.env.VITE_APP_ID,
  measurementId: import.meta.env.VITE_MEASUREMENT_ID,
};

const FireBaseApp = initializeApp(firebaseConfig);

export default FireBaseApp;
```

## Adding routing

In this section we will be focusing on setting up the routing so we can switch through pages. the end result of this blog / app will be a sign-up and login page with a password reset email feature, when the user is logged in they will be redirected to a home page. That is the goal we are trying to reach for here, with some fancy smancy styling added to it using tailwind.

lets start by adding react router to the project.

Open up a terminal session if its not still open and go to your react project folder and install the react router package as a depency by running this npm install command:

`npm install react-router-dom`

When the installation is done lets head over to the main.tsx file where we will modify the code. Currently we only have one page with some Vite content running and we want more then one page in our app. The react router can be configured in two ways:

1. We us the createBrowserRouter function that takes an array of objects as an argument. The objects hold all different paths and the corresponding components that render content when the route is requested by the user. The createBrowserRouter will be passed to the RouterProvider component inside React render function.
2. The other way is using the  createRoutesFromElements and pass this as an argument to the createBrowserRouter function. this way you don't pass routes as an object that represents all routes in your app but you can use React component.

Enough talk here is the code snippet for the routing just paste it in the `main.tsx` file.

```typescript
import React from "react";
import ReactDOM from "react-dom/client";
import { createBrowserRouter, RouterProvider } from "react-router-dom";
import "./index.css";
import App from "./App";

const router = createBrowserRouter([
  {
    path: "/",
    element: <App />,
  },
]);

ReactDOM.createRoot(document.getElementById("root") as HTMLElement).render(
  <React.StrictMode>
    <RouterProvider router={router} />
  </React.StrictMode>
);

```

Lets go ahead and create two more folders in our project we will have a pages and components folder.
create both folders inside the src folder and an `index.ts` file in each folder. We will use [_Barrels_](https://basarat.gitbook.io/typescript/main-1/barrel) for importing components into another file, this way we only have one file where we will import pages and components from into another folder.

Your project folder structure should by now look something like this.

```bash
src
> assets
> components
    index.ts
> pages
    index.ts
App.css
App.tsx
index.css
main.tsx
vite-env.d.ts
```

What we need to do now is move the App.tsx inside the pages folder and update the index.tsx inside the pages folder like so

```typescript
export { App } from "./App";
```

dont forget to update the import of the App component inside the main.tsx change it to `import App from "./pages/App";`.

lets just add a really basic component inside the App.tsx file to see if it works so far.

```typescript
const App = () => <div>Hello APP!</div>;

export default App;
```

it should render `Hello APP!` in the browser it looks weird in somewhere midway in the browser but lets fix that in the next section.

## Adding some styling with Tailwind

Lets setup up the basics for styling. for this projeft we will be using tailwind, tailwind will provide some fast and easy styling and will help skip explaining a lot of css stuff which is not within the scope of this blog post. Tailwind will give us classes that hold the styling for us so by adding classes to the html elements it will update the styling witjout the use of css as tailwind has done that for us.

so lets set it up and make this app look shiney! 💎✨🪩

so lets go to the tailwind docs and see how we set this up.
go to the docs by clicking this [link](https://tailwindui.com/documentation)

first thing to do is adding tailwind to our package.json and node modules by running this command in the root folder of our project in the commandline

```bash
npm install -D tailwindcss postcss autoprefixer
```

next run

```bash
npx tailwindcss init
```

It will add a tailwind.config.js file.

update the file so it looks like this

```javascript
/** @type {import('tailwindcss').Config} */
export default {
  content: [ "./index.html", "./src/**/*.{js,jsx,ts,tsx}"],
  theme: {
    extend: {},
  },
  plugins: [],
}
```

also create a postcss.config,js file and add this.

```javascript
export default {
    plugins: {
      tailwindcss: {},
      autoprefixer: {},
    },
}
```

next step is updating the index.css file by pasting this code

```css
@tailwind base;
@tailwind components;
@tailwind utilities;

body {
background-color: #f0fdfa;
}
```

when we update the App js file to this 

```javascript
const App = () => (
  <div className="min-h-screen flex justify-center items-center">
    <h1 className="text-3xl font-bold text-blue-600">
      Install & Setup Vite + React + Typescript + Tailwind CSS 3
    </h1>
  </div>
);

export default App;
```

the backround should be slightly green and the text is somwhere in the middle of the screen in a bold sanse serif font. like so:

<ADD IMAGE>

No this is done lets continue on the app itself.

## Setting up firebase Authentication

Just a quick update of what the user experience will be in our app when we are done.

When a user visits the app they will see a signup form with three input fields one for email, one for username and one for the password, it will have a signup button and a link which says login, the link will update the sign up form to a sign in form which only will have an email and password input field as it will imply that the user already has an account so there is no need to submit the username.

When a user signs up or signs in with a correct user name and password the user will be able to login and redirected to the user home page which welcomes theme with there username. Super awesome right! 🤩

first lets enable authentication in firebase for the app.

go to the project overview page of your firebase project on the web and select authentication. press the button where its says get started.

we will only only do email and password for now, so select the email/password option. enable both email/password and email link options and save.


Now lets get coding!

Lets create a an AuthForm component inside the components folder.

and paste in this code

```typescript

import {
  getAuth,
  signInWithEmailAndPassword,
  createUserWithEmailAndPassword,
  sendPasswordResetEmail,
  updateProfile,
} from 'firebase/auth';
import { FormEvent, useState } from 'react';
import { useNavigate } from 'react-router-dom';
import FireBaseApp from '../../firebase';

export const AuthForm = () => {
  const [email, setEmail] = useState('');
  const [password, setPassword] = useState('');
  const [displayName, setDisplayName] = useState('');
  const [signUp, setSignUp] = useState(true);
  const [error, setError] = useState<string | undefined>();
  const auth = getAuth(FireBaseApp);
  const navigate = useNavigate();

  const handleSubmit = async (event: FormEvent) => {
    event.preventDefault();
    if (signUp) {
      try {
        await createUserWithEmailAndPassword(auth, email, password);
        const user = await auth.currentUser;
        if (user) {
          updateProfile(user, {
            displayName,
          });
        }

        navigate('/dashboard');
      } catch (error) {
        console.log(error);
      }
    }

    try {
      await signInWithEmailAndPassword(auth, email, password);
      navigate('/dashboard');
    } catch (error: any) {
      setError(error.code);
    }
  };

  const toggleSignUp = (event: any) => {
    event.preventDefault();
    setSignUp(!signUp);
  };

  return (
    <form
      onSubmit={(E) => handleSubmit(E)}
      className="bg-white p-6 rounded-lg shadow-xl"
    >
      <h1 className="text-center">{signUp ? 'Sign Up' : 'Sign In'}</h1>
      {signUp && (
        <>
          <label
            className="block text-gray-700 font-medium mb-2 text-left"
            htmlFor="username"
          >
            Username
          </label>
          <input
            type="username"
            name="username"
            value={displayName}
            onChange={(event) => setDisplayName(event.target.value)}
            className="w-full p-3 border border-gray-400 rounded-lg outline-teal-500"
            id="username"
            required
          />
        </>
      )}
      <div className="mb-4">
        <label
          className="block text-gray-700 font-medium mb-2 text-left"
          htmlFor="email"
        >
          Email
        </label>
        <input
          type="email"
          name="email"
          value={email}
          onChange={(event) => setEmail(event.target.value)}
          className="w-full p-3 border border-gray-400 rounded-lg outline-teal-500"
          id="email"
          required
        />
      </div>
      <div className="mb-6">
        <label
          className="block text-gray-700 font-medium mb-2 text-left"
          htmlFor="password"
        >
          Password
        </label>
        <input
          type="password"
          name="password"
          value={password}
          onChange={(event) => setPassword(event.target.value)}
          className="w-full p-3 border border-gray-400 rounded-lg outline-teal-500"
          id="password"
          required
        />
      </div>
      <p className="text-red-400">{error && error}</p>
      <button
        type="submit"
        className="bg-teal-500 transition duration-500 hover:bg-teal-600 text-white font-medium w-full p-3 rounded-lg"
      >
        {signUp ? 'Register' : 'Login'}
      </button>
      <div className="flex">
        <div className="flex-start ">
          <button
            className="mr-4 transition duration-500 hover:underline"
            onClick={(e) => toggleSignUp(e)}
          >
            {signUp ? 'Sign In' : 'Sign Up'}
          </button>
          <button
            className="transition duration-500 hover:underline"
            onClick={() => sendPasswordResetEmail(auth, email)}
          >
            Forgot Password
          </button>
        </div>
      </div>
    </form>
  );
};
```

lets zoom a bit in on a view imports in this file and how we use them in a callback
```typescript
import {
  getAuth,
  signInWithEmailAndPassword,
  createUserWithEmailAndPassword,
  sendPasswordResetEmail,
  updateProfile,
} from 'firebase/auth';
```

now add the form to the App.tsx file.
your App.tsx file should look like this:

```typescript
import { AuthForm } from "../components";

const App = () => {
  return (
    <div className="grid place-items-center">
      <div className="w-3/6 max-md:w-full mt-5">
        <AuthForm />
      </div>
    </div>
  );
};

export default App;
```

and also update the main.tsx to add the dashboard route.

```typescript
import React from "react";
import ReactDOM from "react-dom/client";
import { createBrowserRouter, RouterProvider } from "react-router-dom";
import "./index.css";
import App from "./pages/App";
import Dashboard from "./pages/Dashboard";

const router = createBrowserRouter([
  {
    path: "/",
    element: <App />,
  },
  {
    path: "/dashboard",
    element: <Dashboard />,
  },
]);

ReactDOM.createRoot(document.getElementById("root") as HTMLElement).render(
  <React.StrictMode>
    <RouterProvider router={router} />
  </React.StrictMode>
);
```
lets add a new page and create a route for it.
inside the pages folder create a new file called Dashboard.tsx

and paste the following code.

```typescript
import { useEffect } from "react";
import { useNavigate } from "react-router-dom";
import { getAuth } from "firebase/auth";

const Dashboard = () => {
  const auth = getAuth();
  const user = auth.currentUser;
  const navigation = useNavigate();

  useEffect(() => {
    if (user == null) {
      navigation("/");
    }
  }, [user, navigation]);

  return (
    <>
      <div className="container mx-auto m-5">
        <div className="grid grid-cols-1 gap-4">
          <div className="bg-white p-6 shadow-md mb-5 mt-5 transition duration-500 hover:shadow-xl rounded">
            <h2>
              Welcome:{" "}
              <span className="text-lg font-bold">{user?.displayName}</span>
            </h2>
            <hr className="h-px my-8 bg-gray-200 border-0 dark:bg-gray-700"></hr>
            <h3>You are logged IN!!🔥🔥🔥🔥🔥</h3>
          </div>
        </div>
      </div>
    </>
  );
};

export default Dashboard;
```

during the the hackthon we will use a slightly different dashboard page look and will be adjusted to match a todo list app.



## Using the Firebase emulator

So lets kick this one off by telling what it is and why we need it. The Firebase emulator lets us develop locally and have our own firebase local firebase to test and develop new features without bothering our actual firebase project, when we are happy with our new feature we can push and deploy the code so in production we actually use our live firebase project instead of poluting our firbase project with testing data. We will not touch the topic any further in this blog but will be covered during the workshop. We will need it when we start building collections and documemts in the app.

## Firebase Hosting

Lets get this app on the web!

## Final words

Well done! you have succesfully created a React App and added a Firebase project to it.
I hope you enjoyed this blog, i will probably add a follow up after the meetup with more indept things explained, such how to use the FireStore database and create collections and update and delete documents from it. So stay tuned and keep on coding! 🧑‍💻
