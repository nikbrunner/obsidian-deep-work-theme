---
title: Title
created: 12312
---

# Here's what every React Developer needs to know about TypeScript

#typescript

[Here's what every React Developer needs to know about TypeScript - Part 1](https://www.reddit.com/r/reactjs/comments/nn1y6x/heres_what_every_react_developer_needs_to_know/)

[https://blog.dastasoft.com/posts/heres-what-every-react-developer-needs-to-know-about-typescript](https://blog.dastasoft.com/posts/heres-what-every-react-developer-needs-to-know-about-typescript)

> Is it worth using TypeScript with React in 2021? Find out why through examples, you do not need to have any previous TypeScript knowledge.

If you've been using React for a while, you'll have noticed some cases where the freedom and wild nature of JavaScript works against you (and not because of JS 😄), especially if you're working in a team. **You may not know it, but you need TypeScript or at least, you need to test it**.

Let me be clear, I love JavaScript and the freedom it provides, for a long time I was "against" TypeScript.

So I want to go on a journey together, figuring out if TypeScript is worth using or TS is only for people who don't know how to code properly (this was an inside joke in my team a time ago!).

The idea behind this article is to go through the basics of TS and understand the benefits so you can decide if you want those benefits or not, in a second part I will cover the specifics of TS with React.

## Resources

If you want you can go directly to [sample project](https://shopping-list.dastasoft.com/) or [source code](https://github.com/dastasoft/shopping-list) that is a very simple project to test the TypeScript developer experience without Webpack or any other add-ons, just plain TypeScript converted to JavaScript.

The other resources I provide in this article are boilerplates for [React](https://reactjs.org/) and [NextJS](https://nextjs.org/):

- [React TypeScript Boilerplate](https://github.com/dastasoft/react-boilerplate/tree/typescript)
- [Nextjs TypeScript Boilerplate](https://github.com/dastasoft/nextjs-boilerplate/tree/typescript)

If you like programming games, try [PhaserJS](https://phaser.io/) you be able to make games for the browser with TypeScript and it's a fun way to learn TS.

Also be sure to check out [The Official Handbook of TS](https://www.typescriptlang.org/docs/handbook/intro.html) with tons of useful documentation and examples.

[[What is Stoicism?|Stoic]]

## Why ESLint, Prettier and Husky

On the boilerplates I use Airbnb's ESLint rules, Prettier's recommended rules and Husky's pre-commits actions, this will be very useful especially in a team environment where you need everyone to follow the same style of code, but you can also benefit as a solo developer or as a learner.

The Airbnb rules can be strange at some points, but they provide a great explanation and examples so you can decide if the rule makes sense for you or not, and if not you can disable it in the `.eslintrc` file.

I found that for junior profiles or people who are just starting out with JS or TS these rules are very useful, so I recommend you at least try to include them in a project and check the results 😉

## What is TypeScript

[TypeScript](https://www.typescriptlang.org/) or TS is an open source language developed and maintained by Microsoft, TS is also:

- [ ] A multi-paradigm language (like JavaScript).
- [ ] An alternative to JavaScript (more precisely a superset)
- [ ] Allows the use of static types
- [ ] Extra features (generics, interfaces, tuples, etc which will be explained in detail below)
- [ ] Allows for gradual adoption*.
- [ ] Can be used for front-end and back-end development (just like JS)

*You can turn an existing project into a TS project by changing the files one by one, it's not a big bang change.

The browser does not understand TS code, it must be *transcompiled* into JS. JS has a dynamic type mapping value and TS has static types which is less error prone.

In React you already *transcompile* JS with [Babel](https://babeljs.io/), so having to *transcompile* the code is not an extra inconvenience nowadays.

## Why bother dealing with TS?

That's the thing, why bother with TS when you are happy with JS and everything is fine? A while back, as I said before we had an inside joke about languages like TS with types (I was doing Java at the time by the way), that you need types if you don't know how to code correctly.

TypeScript, Java and a bunch of other languages have **static typing** that will define a type associated with a variable and the type will be checked during compile time. Once you define something to be a *string* or a *boolean* you can't change its type.

JavaScript on the other hand has **dynamic typing**, you can assign a string to a variable, and later convert it to a boolean, a number or whatever you want, the type will be dynamically assigned at run time.

But when you look at the TS code on the Internet, you can see...

![Image.jpeg](https://blog.dastasoft.com/_next/image?url=%2Fassets%2Fposts%2Fcontent%2Ftypescript%2Fsyntaxsugar.jpeg&w=3840&q=75)

Syntactic Sugar, syntactic sugar everywhere.

So going back to my team's old joke, yes indeed **it was correct**, if you know exactly what you're doing, you don't need someone constantly telling you that this is a string and only a string, and if at some point it becomes a boolean or something else.... I know what I'm doing!

But the truth is that we are not perfect, and things happen:

- Work in a hurry.
- Having a bad day.
- Leaving an idea on Friday and when you come back on Monday you don't have the same picture of the situation.
- Working in a team, and not everyone has the same level and/or vision.

For the same reasons we use an IDE, IDE extensions, syntax highlighting and linterns instead of the notepad app. TypeScript can fit into these aids.

![Image.jpeg](https://blog.dastasoft.com/_next/image?url=%2Fassets%2Fposts%2Fcontent%2Ftypescript%2Fairbnb.jpg&w=3840&q=75)

Airbnb claims that 38% of bugs on Airbnb could have been prevented by using TypeScript.

### Some mistakes in examples

Let's look at some basic examples with and without TS in the equation:

### Please, I know what I'm using

```jsx
import { MemoryRouter as Router } from 'react-router-dom'

import Routes from './routes'

export default function App() {
  return (
    <Router basename="/my-fancy-app">
      <Routes />
    </Router>
  )
}
```

Do you see anything unusual in the code above? If so, congratulate yourself.

This file was in my boilerplate for a long time, it's not a bug but... `MemoryRouter` doesn't need any `basename` at all. This happens because at some point in the past `BrowserRouter` was used which in fact needs a `basename` property.

With TS you will be notified by `No overload matches this call` which tells you that there is no signature for that component with that property.

**TypeScript not only works as static typing, but it helps you better understand the needs of other libraries,** and by others I mean components and functions from third parties or your co-workers.

Yes I can hear the answer, you must properly know the libraries you are using, and again yes you are right but assuming that everyone involved in a project knows every "external" library and the nuances of the versions can be a daunting task.

### The devil's flag

```js
let isVerified = false;
verifyAmount();


if (isVerified) proceedPayment();
```

I have seen this error many times, I don't have the exact code and each time it has a different nuance but you can get the point, you have a boolean variable that is responsible for letting some code run or not and at some point someone else or maybe yourself in an error, turn the boolean into a string and a non-empty string is a true value.

With TypeScript you would have had the error: `The type 'string' is not assignable to the type 'boolean'` and this error will occur at compile time, even if you don't have your application running at the time, so the chances of the error making it to production are very small.

Again, we can apply the same rule as before, if you code correctly this doesn't happen, if you follow the rules of Clean Code and be careful with what you are doing this can also be avoided, **TypeScript is not meant to allow us to be lazy and disorganized but it can be a good ally**, as syntax highlighting can help to avoid some errors or detect unused variables.

### I though the cat was alive inside that box

```jsx
const MONTH_SELECT_OPTIONS = MONTHS.map((month) => ({
  label: getMonthName(month),
  value: month,
}))

export default function PaymentDisplayer() {
  const [currentMonthFilter, setCurrentMonthFilter] = useState(
    MONTH_SELECT_OPTIONS[0]
  )

  const onChangeHandler = option => {
    setCurrentMonthFilter(option.value)
  }

  return (
    <select onChange={onChangeHandler}>
      {MONTH_SELECT_OPTIONS.map(({ label, value }) => (
        <option key="value" value={value}>
          {label}
        </option>
      ))}
    </select>
  )
}
```

It is very common (and perhaps not recommended) to change the state's type, sometimes it is on purpose like having an `isError` flag and suddenly changing it from boolean false to error message string (and again not recommended at all!), but in other scenarios it is by mistake, like the example above.

The person who wrote this in the first instance thought that in `currentMonthFilter` he would store the actual option of the select, an `HTMLOptionElement` with label and value. Later, the same person on another day or perhaps another developer makes the `changeHandler` and sets the value instead of the full option.

The above example works, and is simplified for learning, but imagine this on a large scale, especially in those components where actions are passed underneath as props.

Here TypeScript would help us in two ways:

- Static typing will throw an error when trying to change the type of `currentMonthFilter` from `{label: string, value: number}` to `number`.
- The person coding the next step of calling a service to retrieve payments with that filter will know through *IntelliSense* what type they will get from the state and whether it matches the type the service needs.

**So TypeScript also allows us to inspect from the IDE the different functions, parameters and documentation of third-party libraries and components of our peers**.

Through these examples (which are perhaps not too representative to be honest) we can conclude that TypeScript tries to help us in a React environment with:

- Being coherent in typing and consistent with static types
- Providing documentation and *IntelliSense* of the available possibilities
- Detecting bugs early

## Setup TypeScript

In this article we will use the Global Installation, because I think it is better to first dive into TypeScript in isolation without any Webpack, React or any other variables and see how it works and what problems it solves, but let's see how to install in the different environments:

### Installation with CRA (Create-React-App)

- You can use the CRA template for TS with `yarn create react-app my-app --template typescript`
- You can use the ready-to-go boilerplate provided in the resources section.

If it is an existing project, you can use the following command, and convert your js files to ts/tsx files.

```shell
npm install --save typescript @types/node @types/react @types/react-dom @types/jest
```

### Installation with Nextjs

- If you install TypeScript as a dependency, Nextjs will create a `tsconfig` file for you once you start it.
- If you create a `tsconfig` file, Nextjs will provide instructions for installing TypeScript into the project once you start it.
- You can use the ready-to-use boilerplate provided in the resources section.

```shell
npm install --save typescript @types/node @types/react @types/react-dom @types/jest
```

### Global Installation

```shell
npm install -g typescript
```

### TypeScript Compiler (tsc)

Once you have installed TypeScript on your system or with any of the other options mentioned above, you can use the TypeScript compiler, the `tsc` command.

Let's test the compiler with the minimum configuration:

#### Steps

- Create a new empty folder
- Place an `index.html` with the basic HTML5 structure inside.
- Create an empty `index.ts` file at the same level as `index.html`.
- Open a terminal and type `tsc --init` (assuming you have installed global typescript) this will create for you a `tsconfig.json` (we will look at this file in detail in the next section).

##### Example

You will have something like this:

```other
- index.html
- index.ts
- tsconfig.json
```

###### Another one

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta http-equiv="X-UA-Compatible" content="IE=edge" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>
  </head>
  <body></body>
</html>
```

Now you need to include the ts file in the HTML but, browsers don't understand TypeScript they understand JavaScript, so you can modify your `index.html` to:

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta http-equiv="X-UA-Compatible" content="IE=edge" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>
  </head>
  <body></body>
  <script src="./index.js"><script/>
</html>
```

Open a new terminal and type `tsc`. Your `index.ts` file will be converted into an `index.js` that the browser can read.

Instead of typing the `tsc` command every time you want to compile the TS file into a JS file, you can put TypeScript in watch mode with `tsc -w`.

Now my recommendation is that you open both TS and JS files side by side, and type regular JS into the `index.ts` file, and test what the outputs are. (We'll use this a lot in the next sections to test what TS generates).

![Image.png](https://blog.dastasoft.com/_next/image?url=%2Fassets%2Fposts%2Fcontent%2Ftypescript%2Fside-by-side.png&w=3840&q=75)

Do some test using tsc -w option

### tsconfig.json

If you are following the article, you have created this file with the `tsc --init` command which creates the `tsconfig.json` with some default configuration and a bunch of comments which are great to start with.

Let's look at some of the properties that might be useful to get you started:

- `target` is the version of JS we are converting our TS code to, depending on the browsers you want to support you may need to set some older version. It can be a good learning resource too, try playing with different versions and see what JS code is generated.
- `module` defines what kind of syntax you will use for modules, `commonjs` which is the default uses `require/module.exports` and modern JS (ES6+) uses `import/export`.*
- `lib` In React and Nextjs boilerplates I use this setting, you need it to specify additional libraries you will use in your project and check additional types, e.g. DOM related.
- `jsx` In React you will need to set it to at least `preserve` this mode assumes that another tool will compile that part (Babel in this case) but TSC will do the type checking.**
- `outDir` where the files will be placed after the compilation, for example in most React projects it will be placed in a `build` folder.
- `rootDir` where the files will be taken for compilation, on most React projects this will be `./src`
- `strict` enables a set of rules for type checking which results in a stronger check for what is considered "correct", I recommend starting with this on false when you are learning and when you feel confident enough turn it on and check what new red flags you have, but remember you will get the full potential of TS with this option enabled. This option also enables all the strict options below, which you can disable individually.
- `include` the folder(s) you want to include to be compiled, for example the `src` folder
- `exclude` the folder(s) you want to prevent from being compiled, for example the `node_modules` folder.

*If you want to use `import/export` you need to change `target` to ES6 or higher, in the example project we will use this syntax so check the rest of the article for this.

**You can set this property to `react` or `react-native` this is used if you want TSC to compile your JSX code into regular JS code, in most cases we will leave this property to `preserve` which will send the file as regular JSX and Babel/Webpack will do the rest.

In the sample project for this article, we will take the files `rootDir` from `./src` and will place it `outDir` in `public` folder.

## Shopping List

![Image.png](https://blog.dastasoft.com/_next/image?url=%2Fassets%2Fposts%2Fcontent%2Ftypescript%2Fshopping-list.png&w=3840&q=75)

Shopping List in a desktop web browser

The sample project is very basic stuff, you can insert different items and their quantities in different sections and later you can remove them while you shop and check what you have to buy next.

The idea behind this example project is to get used to TypeScript and the general workflow, because once you get into the React environment a lot of the magic is done for you by Webpack or any other bundler, so I think it's important to know the very basic things and later enjoy the work that the bundler does for us.

Let's see what we can use from TS to get a better, less error-prone code base.

### Modules

If you want to use ES6 `import/export` modules you must configure `tsconfig` with:

- **target**: es6 or higher
- **module**: es2015 or more

And in the `index.html` file you must add the module type:

```html
<script type="module" src="app.js"></script>
```

However, the use of modules has two drawbacks:

- Compatibility with older browsers is less likely.
- Files in production will be split, so you will have multiple requests for each file (this can be fixed by using a bundler like Webpack).

### Types

In JavaScript types are assigned at runtime, when the interpreter sees your variable and the value, it decides what type it is, so we can do things like this:

```js
let job = "Warrior"; 
let level = 75; 
let isExpansionJob = false; 

level = "iLevel" + 75
```

In TypeScript types are assigned at compile time, so once the type is defined it will be protected under that signature.

```ts
let job: string = "Samurai";
let level: number = 75;
let isExpansionJob: boolean = true;

level = "iLevel" + 75
```

#### Inference

In fact, it is not necessary to explicitly state the type you want the variables to be, TS can infer the type by their value.

```js
let job = "Samurai";
let level = 75;
let isExpansionJob = true;

level = "iLevel" + 75
```

In React, which we'll look at in Part 2 of this article in detail, you'll see the inference as well, for example in `useState`

```jsx
const [currentMonthFilter, setCurrentMonthFilter] = useState("January")

useEffect(() => {
   setCurrentMonthFilter(1) 
}, [])
```

#### Any and Unknown

I have said all along that the TS has static types, but there is a nuance to that statement.

```ts
let level: any = 10;

level = "iLevel" + 125; 


level = false;
```

Welcome back to JavaScript! `any` is a dynamic type for when you don't know what type the variable will be in the future but it somehow reverses all the advantages that TS provides.

```ts
let level: any = 10;

level = "iLevel" + 125;

level = false;

let stringLevel: string = level;
console.log(typeof stringLevel);
stringLevel.replace("false", "true");
```

When you assign `level` to `stringLevel` of type `string` it does not become a string, it is still a boolean, so the `replace` function does not exist and the code fails at runtime. `Uncaught TypeError: stringLevel.replace is not a function`

For that we have another type which is the safe counterpart of `any` type:

```ts
let level: unknown = 10;

level = "iLevel" + 125;

level = false;

let stringLevel: string = level;
```

With `unknown` you can assign any type as in `any` but this time the compiler gets the error when you try to assign to another type. So if you don't know what type it will be, try using

