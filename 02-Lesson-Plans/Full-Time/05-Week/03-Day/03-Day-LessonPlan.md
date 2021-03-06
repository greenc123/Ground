## 5.3 - Constructors (10:00 AM)

## Instructors and TAs: Please take the Mid-Course Instructional Staff Survey

Trilogy as a company values transparency and data-driven change quite highly. As we grow, we know there will be areas that need improvement. It’s hard for us to know what these areas are unless we’re asking questions. Your candid input truly matters to us, as you are vital members of the Trilogy team. In addition to the individual feedback at the end of lesson plans
we would appreciate your feedback at the following link:
[https://docs.google.com/forms/d/e/1FAIpQLSdWXdBydy047_Ys1wm6D5iJY_J-0Mo0BqCjfGc4Er2Bz9jo5g/viewform](https://docs.google.com/forms/d/e/1FAIpQLSdWXdBydy047_Ys1wm6D5iJY_J-0Mo0BqCjfGc4Er2Bz9jo5g/viewform)

## Overview

In this class we will be introducing students to the usage of JavaScript constructors and how they can be used to dynamically create objects with similar schemas.

## Instructor's Notes

* `Summary: Complete activity 40 in Unit 9 and 1-6 in Unit 10`

* Constructors are extremely useful in creating objects of similar types and allow for the development of very interesting applications. Make sure your students have a firm understanding of how objects function.

## Learning Objectives

* By the end of class students will be able to...

  * Use OOP to create a banking application.

  * Use constructor functions to create new objects.

  * Use object prototypes to add methods to objects.

## Slides

[5.3 Intro to OOP Slide Deck](https://docs.google.com/presentation/d/1k9lO6jSIGGYNRDKULu6O1glKQzyvaPTIkRSRnIWbbqg/edit?usp=sharing)

## Time Tracker

[5.3 Time Tracker](https://docs.google.com/spreadsheets/d/1f1TNdbe2vCC49YpQJ5l1r8O-6Vg3KCPh90WLM8yNNIs/edit?usp=sharing)

- - -

## Class Instruction

### 1. Instructor Do: Introduce Mini Project (5 mins)

* Run the mini-project solution found in [40-Stu_Mini-Project/Solved/Bonus/index.js](../../../../01-Class-Content/09-NodeJS/01-Activities/40-Stu_Mini-Project/Solved/Bonus/index.js) without demonstrating any of the code.

* Answer any questions students may have about the intended functionality of this completed activity.

### 2. Students Do: Mini Project (60 mins)

* Direct students to the instructions found in [40-Stu_Mini-Project](../../../../01-Class-Content/09-NodeJS/01-Activities/40-Stu_Mini-Project/README.md):

```md
# Mini Project

In this activity, you will build a command-line tool that generates and HTML portfolio page from user input.

## Instructions

* Your application should prompt the user for information such as their name, location, bio, LinkedIn URL, and GitHub URL. Feel free to add any additional prompts you think of.

* Using the data collected from the prompts, an HTML document should be constructed containing this information and written to the filesystem. Be sure to add some CSS styling to the document.

* Some tools and technologies you'll need to accomplish this:

  * FS: For writing to the filesystem
  * Inquirer: For collecting user input
  * String template literals: For generating a string version of the HTML document before it is written to the filesystem
  * Promises: For handling asynchronous behavior

## Hint(s)

* It may be a good idea to start building out the HTML skeleton in a real HTML file. Once you're happy with the HTML file's appearance in the browser, you can copy/paste its contents into a string template literal and write a function to insert the user input into the appropriate places in the HTML string before writing it to the filesystem.
```

### 3. Instructor Do: Review Mini Project (10 mins)

* Use the prompts and talking points below to demonstrate the following key point(s):

  * ✔️ We can construct an HTML string using string template literals

  * ✔️ We can use Promises or the async/await syntax to control the flow of asynchronous code

* Open [40-Stu_Mini-Project/Solved/Basic/index.js](../../../../01-Class-Content/09-NodeJS/01-Activities/40-Stu_Mini-Project/Solved/Basic/index.js) in your IDE and point out the following:

  * First we require the necessary packages.

  ```js
  const inquirer = require("inquirer");
  const fs = require("fs");
  const util = require("util");
  ```

  * 📝 We use the `util.promisify` method to take a function that uses Node style callbacks to create a new version of the function that now uses Promises.

  ```js
  const writeFileAsync = util.promisify(fs.writeFile);
  ```
  
  * We prompt the user for their basic information using `inquirer.prompt`.

  ```js
  function promptUser() {
    return inquirer.prompt([
      {
        type: "input",
        name: "name",
        message: "What is your name?"
      },
      {
        type: "input",
        name: "location",
        message: "Where are you from?"
      },
      {
        type: "input",
        name: "hobby",
        message: "What is your favorite hobby?"
      },
      {
        type: "input",
        name: "food",
        message: "What is your favorite food?"
      },
      {
        type: "input",
        name: "github",
        message: "Enter your GitHub Username"
      },
      {
        type: "input",
        name: "linkedin",
        message: "Enter your LinkedIn URL."
      }
    ]);
  }
  ```

  * We create a function named `generateHTML` that returns a `template literal` which in this case is just an HTML document. Within this template literal we can insert the responses we gathered from our user via inquirer.

  ```js
  function generateHTML(answers) {
    return `
  <!DOCTYPE html>
  <html lang="en">
  <head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/4.0.0/css/bootstrap.min.css">
    <title>Document</title>
  </head>
  <body>
    <div class="jumbotron jumbotron-fluid">
    <div class="container">
      <h1 class="display-4">Hi! My name is ${answers.name}</h1>
      <p class="lead">I am from ${answers.location}.</p>
      <h3>Example heading <span class="badge badge-secondary">Contact Me</span></h3>
      <ul class="list-group">
        <li class="list-group-item">My GitHub username is ${answers.github}</li>
        <li class="list-group-item">LinkedIn: ${answers.linkedin}</li>
      </ul>
    </div>
  </div>
  </body>
  </html>`;
  }
  ```

  * Finally we call our `promptUser` function and on success we generate our HTML file with this customized responses. We then create the file, appending the contents of the HTML template literal we created.

  ```js
  promptUser()
    .then(function(answers) {
      const html = generateHTML(answers);

      return writeFileAsync("index.html", html);
    })
    .then(function() {
      console.log("Successfully wrote to index.html");
    })
    .catch(function(err) {
      console.log(err);
    });
  ```

* Answer any high-level questions before demonstrating the bonus solution using async/await.

* Open [40-Stu_Mini-Project/Solved/Bonus/index.js](../../../../01-Class-Content/09-NodeJS/01-Activities/40-Stu_Mini-Project/Solved/Bonus/index.js) in your IDE and point out the following differences:

  * Code using the `await` syntax must be inside of a function declared with the `async` identifier. We're also using a `try/catch` block to handle any errors that may occur when using async/await.

  ```js
  async function init() {
  console.log("hi")
  try {
    const answers = await promptUser();

    const html = generateHTML(answers);

    await writeFileAsync("index.html", html);

    console.log("Successfully wrote to index.html");
  } catch(err) {
    console.log(err);
  }
  ```

* Ask the class the following question(s) and call on students for the corresponding answer(s):

  * ☝️ So, how can asynchronous code help developers write better code.

  * 🙋 Asynchronous programming allows our code to execute logic without blocking the rest of the applications functionality.

- - -

### 4. Review Mini Project and Unit 09 (30 mins)

* Continue to answer students lingering questions about the Mini Project. If no questions arise, lead a review session on unit 09 until the break.

### 5. BREAK (30 mins)

- - -

### 6. Instructor Do: Welcome Class (5 mins)

* Welcome students to class.

* Open the [slide deck](https://docs.google.com/presentation/d/1k9lO6jSIGGYNRDKULu6O1glKQzyvaPTIkRSRnIWbbqg/edit?usp=sharing) and follow these prompts on their corresponding slides:

  * **Constructors (Title)**: Today will be an introduction to OOP, more specifically objects and constructors.

  *  **What is programming?**: So what exactly is programming?

  *  **Programming**: The designing and building of an executable program that will accomplish a specific computing task. Essentially, programming is problem solving.

  *  **What problems do we solve?**: What problems do programmers solve?

  * **Algorithms and Automation**: Programming allows for us to solve almost any task or problem on a computer. There are two primary categories: Algorithms and Automation.

  * **What is DRY?**: What does DRY mean?

  * **DRY**: Dry means “Don’t Repeat Yourself!”. Rewriting code wastes time, memory, and can confuse later readers and/or contributors to your code.

  * **What is an object?**: What is an object?

  * **Objects**: Objects in JavaScript are unordered collections of related data built on a key:value structure where values can be any data-type, including functions. 

  * **Why are Objects important in Javascript**: What makes objects so important?

  * **Everything is an object!**: Well, almost everything. Arrays, Date, Math, even functions are objects! Primitive types are NOT objects.

  * **What is object-oriented programming?**: What is OOP?

  * **OOP**: OOP is a programming paradigm or pattern of programming centered around objects. Problems are thought of in a way in which a collection of objects work together to solve a problem.

* Ask the class the following question(s) and call on students for the corresponding answer(s):

  * ☝️ So, what do you think we are going to do today?

  * 🙋 Program some objects!

### 7. Students Do: Raining Cats and Dogs (15 mins)

* Direct students to the activity found in [01-Stu_Cats-And-Dogs](../../../../01-Class-Content/10-OOP/01-Activities/01-Stu_Cats-And-Dogs)

```md
# Cats and Dogs

In this activity you will make a cat object and dog object each with three keys.

## Instructions

* Make a dogs object with three keys:

  * First key called "raining" with a value of true

  * Second key called "noise" with a value of "Woof!"

  * Third key called "makeNoise" which contains a function which console.logs the noise to the screen if it is raining dogs

* Next make a cats object with three keys:

  * First key called "raining" with a value of false

  * Second key called "noise" with a value of "Meow!"

  * Third key called "makeNoise" which contains a function which console.logs the noise to the screen if it is raining cats

* Make the dog bark

* Make the cat meow

## 🏆BONUS

* Create a function called "massHysteria" which takes in both the cats and the dogs object and prints "DOGS AND CATS LIVING TOGETHER! MASS HYSTERIA!" if both of the `raining` keys are equal to true.

* See if there is anyway to further optimize your code.
```

### 8. Instructor Do: Review Raining Cats and Dogs (5 mins)

* Use the prompts and talking points below to demonstrate the following key point(s):

  * ✔️ Keys can have the value of a function

  * ✔️ Dot notation can be used to modify objects and call methods.

* Open [01-Stu_Cats-And-Dogs Solved/script.js](../../../../01-Class-Content/10-OOP/01-Activities/01-Stu_Cats-And-Dogs/Solved) in your IDE and explain the following points to students:

  * 🔑 We create a `makeNoise` key and give it the value of a function.

  ```js
  makeNoise: function() {
    if (this.raining === true) {
      console.log(this.noise);
      }
    }
  };
  ```

  * 🔑 We use dot notation to call methods contained in our object.

  ```js
  dogs.makeNoise();
  ```

  * 🔑 We can change the value of a key using dot notation as well.

  ```js
  cats.raining = true;
  ```
  
  * We create a function `massHysteria` which will take in a dogs object and cats object and check that BOTH have a key:value of `raining: true`. 

  ```js
  const massHysteria = function(dogs, cats) {
  if (dogs.raining === true && cats.raining === true) {
    console.log("DOGS AND CATS LIVING TOGETHER! MASS HYSTERIA!");
    }
  };
  ```

  * Finally we invoke our function passing in our two objects.

  ```js
  massHysteria(dogs, cats);
  ```

* Ask the class the following question(s) and call on students for the corresponding answer(s):

  * ☝️ What if we wanted to create multiple different animal objects from a blueprint?

  * 🙋 We can use a constructor function to create objects based on a structure we specify.

* Use the discussion to transition to the next topic.

### 9. Instructor Do: Cats and Dogs with Constructors! (10 min)

* Use the prompts and talking points below to demonstrate the following key point(s):

  * ✔️Constructor functions are capitalized and can take in parameters.

  * ✔️The `new` keyword invokes is used to create new objects.

* Ask the class the following question(s) and call on students for the corresponding answer(s):

  * ☝️ What difference do you from the previous activity we worked on?

  * 🙋 We create a constructor function called `Animals`, instead of individual cat and dog objects.

  * ☝️ Why is `Animal` upper-cased. ️

  * 🙋 It is a common naming convention to upper-case the names of constructor functions, as well as classes.

* Open [02-Ins_Cats-And-Dogs-Constructors](../../../../01-Class-Content/10-OOP/01-Activities/02-Ins_Cats-And-Dogs-Constructors/rainingCatsAndDogs-con.js) in your IDE and explain the following points:

  * 🔑 We first declare a constructor function named `Animal`. It will take 2 parameters which will be passed into our keys as their value.

  ```js
  function Animal(raining, noise) {
    this.raining = raining;
    this.noise = noise;
  ```

  * We give our object a key of `makeNoise` thats value is a function. The function checks if the `raining` key's value is `true`. If it is `console.log` the value of the key `noise`.

  ```js
  this.makeNoise = function() {
    if (this.raining === true) {
      console.log(this.noise);
    }
  };
  ```

  * 🔑 We create a new object via our constructor function using the `new` keyword. We pass in the values we want our keys to have as arguments to the constructor.

  ```js
  var dogs = new Animal(true, "Woof!");
  var cats = new Animal(false, "Meow!");
  ```

  * We can now invoke the `makeNoise` method on our created objects.

  ```js
  dogs.makeNoise();
  cats.makeNoise();
  ```

* Ask the class the following question(s) and call on students for the corresponding answer(s):

  * ☝️ How are constructors useful?

  * 🙋 They allow us to create as many objects as we want, all from a single blueprint. This lessens redundant code.

* Use the discussion to transition to the next activity.

### 10. Student Do: MiniBank (20 mins)

* Direct students to the next activity, found in [03-Stu_Mini-Bank](../../../../01-Class-Content/10-OOP/01-Activities/03-Stu_Mini-Bank/Unsolved).

```md
# MiniBank

In this activity you will use objects to create a mini banking application.

## Instructions

Update the `createMiniBank` function to achieve the following:

1. Add another private value of `statement` that should be set to an array with one value, `0`. This array will contain all transactions made with the MiniBank objects.

2. Add a `setBalance` function that takes a value and updates the private `statement` value to it.

3. Write an `updateStatement` function that takes in a number and pushes it to the `statement` array.

4. Write a `getStatement` function that returns the `statement` array.

5. Write a `printStatement` function that prints each element in the in the `statement` array on its own line.

6. Write a `deposit` function that takes a value and updates the `balance` value using the `setBalance` function.

7. Write a `withdraw` function that takes a value and subtracts it from the `balance`.

8. Return the `printBalance`, `printStatement`, `deposit`, `withdraw` functions from the `createMiniBank` function.

* Then, create a new `minibank` object using the `createMiniBank` function.

1. Print the `minibank` balance.

2. Deposit some money into the `minibank` object.

3. Print the `minibank` balance.

4. Withdraw some money from the `minibank` object.

5. Print the `minibank` balance.

## Bonus 🏆

* Add code to throw an error if the user tries to withdraw more money than they have, or try to deposit or withdraw values that aren't positive numbers.

* Add code to return a copy of the `statement` when `getStatement` is called, rather than returning the original array.
```

### 11. Instructor Do: Review Mini-Bank (10 mins)

* Use the prompts and talking points below to demonstrate the following key point(s):

  * ✔️ `MiniBank` is made via creating a constructor function.

  * ✔️ We use `typeof` to make sure we get the inputs we want.

* Open [03-Stu_Mini-Bank/Solved](../../../../01-Class-Content/10-OOP/01-Activities/03-Stu_Mini-Bank/Solved/minibank.js) in your IDE and explain the following points to students:

  * 🔑 We create a constructor function called `MiniBank` that will take in one argument, the starting balance.

  ```js
  function MiniBank(balance) {
  ```

  * We use `this.` notation to declare functions on our constructor so all objects created from this constructor have access to those methods.

  ```js
  this.getBalance = function() {
    return this.balance;
  },
  ```

  * 🔑 In our `deposit` function we first check to make sure the provided argument is a number, and the number is greater than 0. We then set our `newBalance` to be equal to our current balance plus the current given value of the deposit.

  ```js
  this.deposit = function(value) {
  if (typeof value !== "number" || value <= 0) {
    throw new Error("'value' must be a positive number!");
  }
  var newBalance = this.getBalance() + value;
  ```

  * After we have gotten our `newBalance` we invoke the `setBalance` function and `updateStatement()` function. Finally we console log the deposited value.

  ```js
  this.setBalance(newBalance);
  this.updateStatement(newBalance);
  console.log(`Deposited ${value}!`);
  },
  ```

  * To create our new mini bank via our constructor we invoke the constructor using the `new` keyword.

  ```js
  var bank = new MiniBank(0);
  ```

  * Now we can call any of the functions we coded into the constructor earlier.

  ```js
  bank.printBalance();
  bank.deposit(85);
  bank.printBalance();
  bank.withdraw(20);
  bank.printBalance();
  bank.printStatement();
  ```

* Ask the class the following question(s) and call on students for the corresponding answer(s):

  * ☝️ How does OOP make solving this activity easier?

  * 🙋 The use of objects and constructors allows us to create a single blueprint, that we can then use to create as many instances of our `MiniBank` as we like.

### 12. Student Do: Weather Admin (15 mins)

* Direct students to the next activity located in [04-Stu_Weather-Admin/Unsolved](../../../../01-Class-Content/10-OOP/01-Activities/04-Stu_Weather-Admin/Unsolved).

```md
# Weather Admin

In this activity you will create a CLI based weather application that will give updates about the weather at the searched location.

## Instructions

* There are two ways to run this app: as a user or as an administrator. To run as admin, type `node CLI.js admin` To run as a user, type `node CLI.js user bob "los angeles, ca"`, where the user mode must provide a name and a location as additional arguments.

* Ideally, users can search for weather data about a location. Every time they search, the place and time get saved to a text file. So when the app is run as admin, the admin user can see a history of all searches.

* Unfortunately, this app is also broken. Try to run it as either a user or admin and fix the actual errors that appear in the console and then any "logic" errors that prevent the expected behavior.
```

### 13. Instructor Do: Review Weather Admin (10 mins)

* Use the prompts and talking points below to demonstrate the following key point(s):

  * ✔️ NPM packages are brought in with `require`.

  * ✔️ `process.argv` allows us to grab CLI arguments.

* Open [04-Stu_Weather-Admin/Solved](../../../../01-Class-Content/10-OOP/01-Activities/04-Stu_Weather-Admin/Solved) in your IDE and explain the following to students:

  * 🔑 First we require the npm package `weather-js`.

  ```js
  const weather = require("weather-js");
  ```

  * We create a constructor function called UserSearch that will take a `name` and `location` as arguments. It will also use `Date.now();` to get the current date.

  ```js
  const UserSearch = function(name, location) {
  this.name = name;
  this.location = location;
  this.date = Date.now();
  ```

  * Our constructor also has a method of `getWeather`.  It will make use of the `weather-js` search function to search for weather of a given location. Lastly, we export our `UserSearch` constructor.

  ```js
  this.getWeather = function() {
    weather.find({ search: this.location, degreeType: "F" }, function(err, result) {
      if (err) {
        console.log(err);
      }
      console.log(JSON.stringify(result, null, 2));
      });
    };
  };

  module.exports = UserSearch;
  ```

* Open [04-Stu_Weather-Admin/Solved/WeatherAdmin.js](../../../../01-Class-Content/10-OOP/01-Activities/04-Stu_Weather-Admin/Solved/WeatherAdmin.js) in your IDE and explain the following points:

  * First we require all the pieces necessary. `fs` is the File System, allowing us to create, delete, or update files on a users local machine. `UserSearch` is our constructor function we exported from `UserSearch.js`. Finally, we import `moment`, an NPM package for dates and times.

  ```js
  var fs = require("fs");
  var UserSearch = require("./UserSearch");
  var moment = require("moment");
  ```

  * We create a constructor function called `WeatherAdmin`. It is given a method of `getData` which will use the file system to read a `log.txt` file if it exists, and log that data to the console.

  ```js
  var WeatherAdmin = function() {
  this.getData = function() {
    fs.readFile("log.txt", "utf8", function(error, data) {
      console.log(data);
    });
  };
  ```

  * `WeatherAdmin` also gets a method of `newUserSearch`. This method takes in two arguments, name and location, much like our `UserSearch` constructor. This is so we can pass those two arguments along into the `UserSearch` constructor, as this method will instantiate a new `UserSearch` object and save it to a variable of `newUserSearch`.

  ```js
  this.newUserSearch = function(name, location) {
    var newUserSearch = new UserSearch(name, location);
  ```

  * We set our `logTxt` variable to equal a string we build that will display the name, location, and date of the search. We then call `moment` to get the date, and format it to `MM-DD-YYYY`.

  ```js
  var logTxt =
  "\nName: " +
  newUserSearch.name +
  " Location: " +
  newUserSearch.location +
  " Date: " +
  moment(newUserSearch.date).format("MM-DD-YYYY");
  ```

  * We use the `fs.appendFile` method to append the current value of `logTxt` to our `log.txt` file.

  ```js
  fs.appendFile("log.txt", logTxt, function(err) {
    if (err) throw err;
  });
  ```

  * Next we call the `getWeather` method on our `newUserSearch` object.

  ```js
  newUserSearch.getWeather();
  ```

  * Finally we export our `WeatherAdmin` constructor.

  ```js
  module.exports = WeatherAdmin;
  ```

* Open [04-Stu_Weather-Admin/Solved/CLI.js](../../../../01-Class-Content/10-OOP/01-Activities/04-Stu_Weather-Admin/Solved/CLI.js) in your IDE and explain the following points:

  * 🔑First we require our WeatherAdmin export from `WeatherAdmin.js`

  ```js
  const WeatherAdmin = require("./WeatherAdmin");
  ```

  * 🔑 We use `process.argv`, taking the 3rd argument to find out if the value is `admin` or `user`.

  ```js
  const loginType = process.argv[2];
  ```

  * ✔️ We also need `Users` to provide a name and location.

  ```js
  const userName = process.argv[3];
  const userLocation = process.argv[4];
  ```

  * We create an instance of the `WeatherAdmin`. If they are running it as an `admin`, run the `getData` method. If they are not an `admin`, we will run `newUserSearch` passing the arguments from the command line.

  ```js
  const myAdmin = new WeatherAdmin();

  if (loginType === "admin") {
    myAdmin.getData();
  }
  else {
    myAdmin.newUserSearch(userName, userLocation);
  }
  ```

### 14. Instructor Do: Introduce Prototypes (15 mins)

* Use the prompts and talking points below to demonstrate the following key point(s):

  * ✔️ Objects, arrays, and primitives all have a `.prototype.`

  * ✔️ The `.prototype.` has methods and properties attached to it.

  * ✔️ Methods declared on the prototype are declared once and memory is allocated for them once, but all objects made from it have access.

  * ✔️ Instance methods only exist on a particular instance of an object, prototype methods are on all instances.

* Open [05-Ins_Prototypes/prototype-demo.js](../../../../01-Class-Content/10-OOP/01-Activities/05-Ins_Prototypes/prototype-demo.js) in your IDE, and then open your browser and the Chrome Dev Tools. Copy and paste each code block into your dev tools and explain the following.

  * We create an array, and console log it. Next, we call the `.forEach` and `.map` methods on it.

  ```js
  myArray = [2, 4, 6, 8];
  console.log(myArray);

  myArray.forEach((num) => console.log(num));

  myArray.map((x) => console.log(x * 2));
  ```

  * 🔑 Next we console log the string `Hello`. We then call `"Hello.toLowerCase"`

  ```js

  console.log("Hello");
  console.log("Hello".toLowerCase());

  console.log(1337);
  console.log((1337).toString());
  ```

* Ask the class the following question(s) and call on students for the corresponding answer(s):

  * ☝️ Where did the `.toLowerCase` come from?

  * 🙋 While those two methods did not show up when we console logged our string, the prototype has these methods built in. Arrays, Objects, even primitives all have a prototype which they take their structure and methods from. Any of of these that you create will have the prototype methods available to it via the `.prototype.`. i.e., `Array.prototype.forEach()`

  * We created a constructor function named `Movie`, which will take in two arguments, `title` and `releaseYear`.

  ```js
  function Movie(title, releaseYear) {
    this.title = title;
    this.releaseYear = releaseYear;
  }
  ```

* Ask the class the following question(s) and call on students for the corresponding answer(s):

  * ☝️ What if we wanted to add a method to our constructor later on in our code?

  * 🙋 We would add that method to the `Movie.prototype`.

  * 🔑 We declare the title of our method, which will be `logInfo`. We do so by typing `Movie.prototype.logInfo = function(){}`. We can only add to our constructor via the object prototype. When we add a method to an object's prototype, all the objects made from it will get it. If there is something that’s going to be the same between objects, and isn’t going to change, it should be on the prototype. If it is defined on the prototype it is only defined once, and memory for it is only allocated once.

  ```js
  Movie.prototype.logInfo = function() {
    console.log(`${this.title} was released in ${this.releaseYear}`);
  };
  ```

  * 🔑 When we create a new object via our `Movie` constructor, it will have access to all the methods defined in the constructor, and those that have been added to its prototype.

  ```js
  const theShining = new Movie("The Shining", 1980)
  theShining.logInfo();
  ```

  * Objects also have their own prototype methods built in. Even though our object was created via a constructor function, it still has access to all the built in object prototype methods.

  ```js
  console.log(theShining.hasOwnProperty('title'));
  console.log(theShining.hasOwnProperty('logInfo'));
  console.log(Movie.prototype.hasOwnProperty('logInfo'));
  ```

* Ask the class the following question(s) and call on students for the corresponding answer(s):

  * ☝️ What does the Object Prototype allow us to do?

  * 🙋 It allows us to reuse properties and methods between objects that need to share them. i.e., all movies can share the same `logInfo` methods, but get their own unique name and release year.

* Use the discussion to transition to the next topic.

### 15. Student Do: RPG Prototype (20 mins)

* Direct students to the next activity, located in [06-Stu_RPG-Prototypes](../../../../01-Class-Content/10-OOP/01-Activities/06-Stu_RPG-Prototypes).

```md
# Character Creation with Prototypes

In this activity you will generate RPG characters using Objects and prototypes.

## Instructions

* Over the course of this activity you are going to be using constructors to create simplistic characters for use within a very basic Roleplaying Game (RPG)

* Each character created using your constructor should have the following properties:

  * Name: The character's name --> String

  * Profession: What the character does for a living --> String

  * Age: The character's age --> Number

  * Strength: Abstraction for how strong the character is --> Number

  * HitPoints (HP): Abstraction for how much health the character has --> Number

  * PrintStats: Function which prints all of a character's properties to the screen

  * Once you have created your constructor, create two new characters and print their properties to the screen

* Now add 3 methods onto it via the prototype.

  * IsAlive: Function which prints whether or not this character is alive by looking into their hitpoints and determining whether they are above or below zero.

  * Attack: Function which takes in a second character and subtracts this character's strength from their hitpoints.

  * LevelUp: Function which increases this character's Age by 1, their Strength by 5, and their HitPoints by 25.
```

### 16. Instructor Do: Review RPG (10 mins)

* Open [06-Stu_RPG-Prototypes/Solved/rpg-prototypes.js](../../../../01-Class-Content/10-OOP/01-Activities/06-Stu_RPG-Prototypes/Solved/rpg-prototypes.js) in your IDE and explain  the following to students:

  * ✔️ We create a `Character` constructor that will take in 6 arguments. We assign those arguments to keys in our constructor.

  ```js
  function Character(name, profession, gender, age, strength, hitpoints) {
    this.name = name;
    this.profession = profession;
    this.gender = gender;
    this.age = age;
    this.strength = strength;
    this.hitpoints = hitpoints;
  }
  ```

  * ✔️ We add an `isAlive` function to our object prototype.

  ```js
  Character.prototype.isAlive = function() {
    if (this.hitpoints > 0) {
        console.log(this.name + " is still alive!");
        console.log("\n-------------\n");
        return true;
    }
    console.log(this.name + " has died!");
    return false;
  };
  ```

  * ✔️ We also add two other functions to our `prototype`. The `attack` method takes in a second object and decreases their "hitpoints" by this character's strength. The `levelUp` method increases `this` character's stats when called.

  ```js
  Character.prototype.attack = function(character2) {
    character2.hitpoints -= this.strength;
  };

  Character.prototype.levelUp = function() {
    this.age += 1;
    this.strength += 5;
    this.hitpoints += 25;
  };
  ```

  * ✔️ Finally we can use our constructor to create two characters, calling their methods from the prototype that we added.

  ```js
  var warrior = new Character("Crusher", "Warrior", "Male", 25, 10, 75);
  var rogue = new Character("Dodger", "Rogue", "Female", 23, 20, 50);

  warrior.printStats();
  rogue.printStats();

  rogue.attack(warrior);
  warrior.printStats();
  warrior.isAlive();

  rogue.levelUp();
  rogue.printStats();
  ```

* Ask the class the following question(s) and call on students for the corresponding answer(s):

  * ☝️  Why don't we just declare the methods in the constructor?

  * 🙋 When we bind a function using the `this` keyword, the method only exists on that instance of the object. For any method bound to `this`, it will be re declared with each new instance of an object.

  * ☝️  How does the prototype help us solve this problem?

  * 🙋 The prototype allows us to declare methods that will be attached to all instances of an object of that prototype. Because the method is applied to the prototype, it is only stored in memory once for all instances.

* Use the discussion to transition to the final activity of the day.

### Lesson Plan Feedback

How did today’s lesson go? Your feedback is important. Please take 5 minutes to complete this anonymous survey.

[Class Survey](https://forms.gle/nYLbt6NZUNJMJ1h38)
