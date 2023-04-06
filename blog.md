# How to create a React app with a Firebase Project ⚛️ 🔥

This post is a walkthrough of how to configure a React with TypeScript project with a Firebase project.
It will touch some basic setup of Firebase for a React App with an example registration login and authorization feature.
There is a link to the repository in the TLDR section. It is meant as a setup for a meetup on how to use React and Firebase in an example app. It can be seen as an encouragement to create your own apps using React and Firebase or explore other frameworks. I hope you will have fun building your own apps using Firebase in the future.

What we will need:

- Node version of at least lts which is currently is v16.18.0
- Your favorite text editor
- A Firebase account
- Also make sure Java is installed on your machine we need it to make the firebase emulator work.
- the latest version of Vite.

What Basic knowlegde this blog expects you to have:

- Some basic commandline knowledge
  - Creating directories
  - Changing directories
  - Running script commands
  - That sort of stuff
- Some Basic React and JS/TS knowledge
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
In the first step give your project any name you like (*please note that a project name is not the same as an app name, later in the process you will add a name for your app.) Next you will be prompted if you would like to use google analytics, we will not disguise the google analytics bit in this blog but you are ofcourse free to select it and explore the feature by yourself.
When you are done creating the project you are now redirected to you project console overview page of the project.

For now we are done with Firebase and will continue after setting up a React project in the __Configure Firebase in the React Project__ part of this Blog.

## Creating a Fresh React project with Vite

This blog will not take a deepdive into Vite we will just touch on how to create a boilerplate react project which will save us a ton of time that we can actually use to create the good stuff.
Just a bit of info on Vite as you can probablky guess by now it can create boilerplate projects and is able to do that for most other popular frontend frameworks its pretty cool to say the least. If you want to know a bit more about it go check out the Vite webpage.

First lets install vite globally so we can easily create boilerplate projects.
To install Vite just run this command in the terminal: `npm i -g vite`
the `-g` flag will make sure its installed globally.

Next find the folder where you would like to store the project and run the next command but with the name of the project of you own choice, I have chosen __firebase_basics_with_react__ but remember to replace the text in the command with your own project name or else you will have a project with the same name.

`npm init vite@latest firebase_basics_with_react -- --template react-ts`

now `cd` inside your folder and run the following commands.

```bash
npm install
npm run dev
```

Now you can visit `http://localhost:5173/` in the browser and your Vite React project in Typescript should be all set.
It is time to use your favorite text editor and start coding!

## Clean up the React project

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

We will just delete all content in the `App.tsx` file and leave the rest for now, everthing will be updated in time.
create a `firebase` folder inside the `src` folder and give in an `index.ts` file.

## Configure Firebase in the React Project

Now that you have a Firebase Project and a fresh new React boilerplate project, lets combine the two and start our app adventure.
Go back to the firebase console of your brand new project.

Now select the button with closing brackets `</>`, this will take us to the place where we can create a web application.

Now you can Register your app and think of a name for it. Leave the hosting checkbox empty for now we will talk about it later.
Next run the npm install command in the terminal on the root folder of your react project.

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

now lets pased the initialize file inside the project. It needs to be inside the index.ts file of the firebase folder.

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

In this section we will be focusing on setting up the routing so we can switch through pages. the end result of this blog / app will be a sign-up and login page with a password reset email feature, when the user is logged in they will be redirected to a home page. That is the goal we are trying to reach for here, with some fancy smancy styling using tailwind.

lets start by adding react router to teh project.

Open up a terminal session if its not still open and go to your react project folder and install the react router package as a depency by running this npm install command:

`npm install react-router-dom`

when the installation is done lets head over to the main.tsx file where we will modify the code. currently we only have one page with some Vite content running and we want more then one page in our app. The react router can be configured in two ways:

1. We us the createBrowserRouter function that takes an array of objects as an argument. The objects hold all different paths and the corresponding components that render content when the route is requested by the user. The createBrowserRouter will be passed to the RouterProvider component inside React render function.
2. The other way is using the  createRoutesFromElements and pass this as an argument to the createBrowserRouter function. this way you dont pas an object that represents alll routes in your app but you can use React component.

Enough talk here is the code snippet for the routing just paste it in the `main.tsx` file.

```typescript
import React from "react";
import ReactDOM from "react-dom/client";
import { createBrowserRouter, RouterProvider } from "react-router-dom";
import "./index.css";
import { App } from "./pages";

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
create both folders inside the src folder and an `index.ts` file in each folder. We will use barrels for importing components into another file, this way we only have one file where we will import pages and components from into another folder.

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

What we need to do now is move the `App.tsx` inside the pages folder and update the index.tsx inside the pages folder like so

```typescript
export { App } from "./App";
```

## Adding some styling with Tailwind

Before we go by creating more pages and components lets add some styling 👨‍🎨.
For this project i have chosen to use Tailwind, i dont want to spend a lot of time styling and it's also not a blog about css, i do like some nice styled apps so this was the choice i went with. You either love Tailwind or hate it, i think it serves well when you want to add style quickly plus you can just ask chat gpt to through you back a nice tailwind styled template.

For those who don't know, Tailwind is not a component library it provides classes we can use to style html elements with.

lets install tailwind by running this command at the root folder of the project.

`npm install tailwindcss`

We do need to add some configuration in our project to make it work.
Go to the index.css file and paste this code in:

```css
@tailwind base;
@tailwind components;
@tailwind utilities;
```

## Setting up firebase Authentication

Ok enough React for now lets head back to the firebase console so we can focus on getting authentication activated.
The cool thing about firebase is that is supports many ways of authentication to give you an example we can use:

- email/ password
- google auth
- facebook
- github
- microsoft
- apple
and even saml open id connect and some more options.

We will add email password for this example.

go to the firebase console and click the authentication link on the left pain, then select get started and select email/password from the options.
switch the toggle to enable and save.
That was it, now lets focus on finishing the app that we will deploy on the web in the firebase hosting so it will be visisble on th web and you can show how awesome you are to your friends!

## Adding the rest of the pages in React

## Using the Firebase emulator

So lets kick this one off by telling what it is and why we need it.

## Firebase Hosting

Lets get this app on the web!

## Final words

Well done! you have succesfully created a React App and added a firebase project to it.
I hope you enjoyed this blog, i will probably add a follow up after the meetup with more indept things explained, such how to use the FireStore database and create collections and update and delete documents from it. So stay tuned and keep on coding! 🧑‍💻
