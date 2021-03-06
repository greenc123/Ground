# 02.5 Lesson Plan: Client-side Storage (10:00 AM) <!--links--> &nbsp; [⬅️](../04-Day/04-Day-LessonPlan.md) &nbsp; [➡️](../../03-Week/01-Day/01-Day-LessonPlan.md)

## Overview

Todays class will be about client side storage APIs and continuing to work on the skills learned prior in the unit.

## Instructor Notes

* This is students first exposure to persistence in their web applications. Its going to be very exciting so use that energy to your advantage. Get students interested in the fact that they can now take their applications to the next level with persistent data.

* The code associated with local and session storage may seem simple, but the concepts and their use cases may be confusing for students seeing these things for the first time. 

* Students will be pseudocoding the ToDo List application at the beginning of class. Familiarize yourself with the code and functionality to more easily help students through this process.

`Complete activities 19-29 in Unit 04`

### Class Objectives

* Use local storage and session storage.

* Describe the differences between the two.

* Identify when client side storage is the right solution.

* Use all of the units concepts to build a persistent ToDo list as well as a Work Timer.

## Slides

N/A

## Time Tracker

* [2.5 Time Tracker](https://drive.google.com/open?id=12Lrx9Wl-DbPaY9m83dvaBBsWIErYDRHRWWgRIbU9WhQ)

- - -

### 1. Instructor Do: Event Delegation Demo (10 min)

* So far we've been manually assigning click listeners to each individual element. Prompt the students with the following question: 

  * What if we need to assign event listeners to several objects on the page? Is there a way that we can easily handle all of these events without duplicating code?

  * Students may reply with a `for` loop. This would work, but it still requires us to create several event listeners.

* Show the students the code in `index.html` but do not open `script.js` yet.

* Then, open `index.html` in the browser and add a couple of items to the shopping cart.

  * Ask the students what JavaScript code they think is necessary for this behavior. Specifically, ask them where they would set up the event listeners.

  * Students may suggest that the event listener is added to each button. 

* Open `script.js` and direct students to the `addEventListener` line. 

  * Note that the event listener was added to the entire list. Then, within the callback, we determine whether or not the clicked item was a button or not by using `event.target.nodeName`. This technique is known as **event delegation**. 

  * Instead of writing a for loop and adding event listeners to every button element, we added an event listener to its parent element. Not only does this simplify our code, but in some cases, it reduces the need for `event.stopPropagation`. Ex: If click events from a child element is triggering the event on its parent, we can set up a conditional that identifies the `target`, then executes a callback accordingly.

  * 🗒 Be sure to mention that event delegation is the technique of listening for events on a parent element, then **delegating** those events differently, depending on the target.

  ```js
  var listEl = document.querySelector("#grocery-list");
  var shoppingCartEl = document.querySelector("#shopping-cart");
  var groceries = ["Bananas", "Apples", "Oranges", "Grapes", "Blueberries"];

  listEl.addEventListener("click", function(event) {
    event.preventDefault();
    if(event.target.matches("button")) {
      var item = document.createElement("div");
      item.textContent = groceries[parseFloat(event.target.parentElement.id)];
      shoppingCartEl.append(item);
    }
  });
  ```

### 2. Student Do: Event Delegation (15 min)

* Direct students to the next activity, found in [19-Stu_Event_Delegation/Unsolved](../../../../01-Class-Content/04-web-apis/01-Activities/19-Stu_Event_Delegation/Unsolved)

```md
# Event Delegation

## Instructions

* In this activity, we are going to create a friends list that allows us to edit information about that friend in a modal.

* Take a moment to study the code in `index.html`. You will not need to add any additional code to this file. Additionally, all of the CSS has been provided.

* In `script.js`, add support the following features: 

  1. When the `Add Person` button is clicked, the person should be added to both the people array and the list elements.

  2. If `edit` is clicked, event delegation should be used to handle the click event.

  3. When the user clicks on edit, the modal should appear with the modal header property already populated with the person's name. If a description exists, the textarea should be populated with the person's description. If not, the description should be left blank.

  4. When the `save` button is clicked, the description of the current person should be updated in the people array.

## Bonus

* Use event delegation to make the modal close if the user clicks away from the modal.
```

### 3. Instructor Do: Review Event Delegation (10 min)

* Take a moment to demonstrate the app, just as you did in the beginning of class.

  * Open [19-Stu_Event_Delegation/Solved/index.html](../../../../01-Class-Content/04-web-apis/01-Activities/19-Stu_Event_Delegation/Solved/index.html) in your browser.

  * Add a person to the list, this time note that doing so adds a person to an array in our JavaScript file, then appends a new HTML element, `li`, to the page.

  * Save the data, then edit a different person. 

  * Return to the person you first editted, and mention that the data is still there.

* Open [19-Stu_Event_Delegation/Solved/script.js](../../../../01-Class-Content/04-web-apis/01-Activities/19-Stu_Event_Delegation/Solved/script.js) and explain the following functions:

  * In `handleClick`, we check to see if the element clicked is a `span`. This is sufficient because this is the only span element on the page at this time.

  * Then we set up variables for the name and description of the person that we clicked on.

  * The `textContent` of the modal header is set to the name and the value of the description textarea is set to any existing description. The logical OR operator ensures that the description is set to an empty string if the value in our array is undefined. 

  ```js
  function handleClick(event) {
    if (event.target.matches("span")) {
      modalEl.style.display = "block";
      currentId = parseInt(event.target.parentElement.id);
      var name = people[currentId].name;
      var description = people[currentId].description;
      modalNameEl.textContent = name;
      descriptionEl.value = description || "";
    }
  }
  ```

  * Remind students that `save` isn't actually persisting the data in a database. We are simply updating the description property of the current person in our people array. This data will remain until we close the browser or reload the page.

  ```js
  saveBtn.addEventListener("click", function(event) {
    event.preventDefault();
    people[currentId].description = descriptionEl.value;
    close();
  });
  ```

  * To setup the behavior to close when the modal is clicked away from, we add an event listener to the document object. We use event delegation to check if the element clicked is the modal container. If so, we close the modal.

  ```js
  document.addEventListener("click", function(event) {
    if (event.target === modalEl) {
      close();
    }
  });
  ```
### 4. Instructor Do: Preview Client Side Storage (5 mins)

* Welcome students to class.

* Open the deployed [ToDo list](https://coding-boot-camp.github.io/fs-ground-04-web-APIs-todo-demo/index.html) in your browser and point out the following to students:

  * We can add items to our todo list.

  * We can complete items on our todo list.

  * When we close the browser tab and reopen the application, we see that our todos are still there. Magic!

* Ask the class the following question(s):

  * When we submit an item to our todo list, where does it go?

  * Our items are stored in the browser. So far none of the applications we've worked on have had any kind of persistent data. When an application was refreshed in the browser, all of its state was reset. 

 * How would we build this application?

  * We would need some kind of storage to hold our data and be able to manipulate that storage via JavaScript.

  * What would allow that?

 * A web API. Specifically client side storage.
 
* Use student answers to transition to the first activity of the day.

### 5. Instructor Do: Todo Local Storage Pseudocode (10 mins)

* Open a new (blank) file in your IDE and lead students in outlining the steps to build the ToDo List application in pseudocode.

* Ask the class the following question(s): 

  * What is the first thing our app needs so a user can submit a new todo?

  * A form and an event listener that listens for new todo submissions when enter is pressed.

  * When a user adds a todo to the HTML form and hits enter, what happens next?
    
  * We dynamically create a new list item, append a "complete" button and append that new list item to the main todo list. Our app also adds the new todo to local storage.

  * What needs to happen when a user clicks the "complete" button?

  * We listen for clicks on the complete button, remove the todo from the page and local storage, then reload the page with the current data.

* Now walk students through the complete pseudocode plan step by step. We need to:

  1. Create an HTML page with a form and an unordered list that holds our list items.

  2. Create an event listener on our form that listens for new submissions.

  3. On submit create a new list item, append a "complete" button and append that new list item to the main todo list.

  4. Save the data to local storage.

  5. Render the current data to the page.

  6. Listen for clicks on the complete button, remove the todo from the page and local storage, then reload the page with the current data.

* Ask the class, "What is the concept we need to learn to build this application?"

  * We do not currently know how to store our data so that it stays in our applications even on close.

* Use students answers regarding storage to transition to the next demo.

### 6. Instructor Do: Demo Local Storage (5 mins)

* Open [20-Ins_Local-Storage-Counter](../../../../01-Class-Content/04-web-apis/01-Activities/20-Ins_Local-Storage-Counter/index.html) in your browser and demo the functionality of the application:

  * When we click either the increment and decrement buttons the number of "hours spent coding" increases or decreases.

* Open your Chrome developer tools, navigate to `Application`, then `Local Storage`, and point out the following: 

  * We are storing our "clicks" with Key/Value pairs.

* Ask the class the following question(s):

  * Is this a database?

  * No, this is **client-side storage**. Data is stored in the client or browser, while a database would require a server.

  * What kind of information would you expect to see stored on the client as opposed to the server?

  * Personal preferences. 
  
  * Shopping cart data. 
  
  * Login session data. Even though user account credentials and information are stored on a server, the client needs to keep track of the fact that the user is still logged in across page refreshes and browser sessions.

  * What would you **not** want to store in the client?

  * Sensitive information, such as credit card numbers, social security numbers and passwords.

* Open [20-Ins_Local-Storage-Counter/script.js](../../../../01-Class-Content/04-web-apis/01-Activities/20-Ins_Local-Storage-Counter/script.js) in your IDE and explain the following: 

  * First we select our counter, add, and subtract buttons and assign them to variables.

    ```js
    var counter = document.querySelector("#counter");
    var addButton = document.querySelector("#add");
    var subtractButton = document.querySelector("#subtract");
    ```
  
  * 🔑 Next set a `count` variable to the current count with the built in local storage method `getItem`. 

    ```js
    var count = localStorage.getItem("count");
    ```

  * The following line of code is what renders the actual count to our webpage. We call `textContent` on our `counter` element and set it equal to `count` from above.

    ```js
    counter.textContent = count;
    ```

  * 🔑 The last piece is to create two event listeners on our `addButton` and `subtractButton` elements. Here we are listening for a click event and calling `count++` or `count--`, which in turn calls our `count` function. We then use the built in local storage `setItem` method to reset our storage with the current data.

    ```js
    addButton.addEventListener("click", function() {
      count++;
      counter.textContent = count;
      
      localStorage.setItem("count", count);
    });

    subtractButton.addEventListener("click", function() {
      count--;
      counter.textContent = count;

      localStorage.setItem("count", count);
    });
    ```

* Ask the class the following question(s): 
  
  * How do we retrieve an item from localStorage?
  
  * `localStorage.getItem()`

  * How do we add an item to localStorage?
  
  * `localStorage.setItem()`
  
* Answer any questions before proceeding to the next activity. 

### 7. Student Do: Local Storage (15 mins)

* Direct students to the next activity, found in [21-Stu_Local-Storage-User/Unsolved](../../../../01-Class-Content/04-web-apis/01-Activities/21-Stu_Local-Storage-User/Unsolved/script.js)

```md
# Local Storage

* You have been provided with a sign up form that successfully submits an email and password. You're job is to write code that saves the email and password to local storage and renders the last submission to the page.

## Instructions

* In your `signUpButton` event listener you will need to:

  * Save the user to localStorage.

* In the `renderLastRegistered()` function you will need to:

  * Fill in code here to retrieve the last registered credentials from local storage.
  
  * If the last registered is null, return early from this function.
  
  * Else set the text of the `userEmailSpan`, `userPasswordSpan` to their corresponding values from local storage.
  
## Hints

* Make sure you call `renderLastRegistered()` after you set your `localStorage`.
```

### 8. Instructor Do: Review Local Storage Activity (5 mins)

* Open [21-Stu_Local-Storage-User/Solved/script.js](../../../../01-Class-Content/04-web-apis/01-Activities/21-Stu_Local-Storage-User/Solved/script.js) and explain the following: 

*  The first thing we need to do is save the form data with:

  ```js
  localStorage.setItem("email", email);
  localStorage.setItem("password", password);
  ```

  * Now that we are persisting data to `localStorage`, let's set and return our last user's data inside our `renderLastRegistered()` function.

  * We access our local storage data and set the email and password to a variable. We then check if the email and password is null and if so, return early.

    ```js
    var email = localStorage.getItem("email");
    var password = localStorage.getItem("password");

    if (email && password === null) {
      return;
    }
    ```

  * If our data is not null, we set the text of the `userEmailSpan` and`userPasswordSpan` to their corresponding values from local storage.

  ```js
  userEmailSpan.textContent = email;
  userPasswordSpan.textContent = password;
  ```

  * With our `renderLastRegistered()` function complete, we can now call it inside our `signUpButton` event listener after we set our `localStorage` when a form is submitted.

  ```js
  localStorage.setItem("email", email);
  localStorage.setItem("password", password);
  renderLastRegistered();
  ```

* Answer any questions before proceeding to the next activity.

### 9. Instructor Do: Local Storage with Uh-oh (5 min)

* Open [22-Ins_Local-Storage-Uh-oh/index.html](../../../../01-Class-Content/04-Web-APIs/01-Activities/22-Ins_Local-Storage-Uh-oh/index.html) in your browser and explain the following: 

  * When we submit our form data, we receive a success response.
  
  * But! When we inspect Local Storage, we see that the value of `user` is `[Object object]`.
 
* Ask the class the following question(s): 

  * What do we think is causing this error? 
  
  * 🔑 We are attempting to store an object in Local Storage. Local storage can only store strings.

* Let's take a look at our code.

* Open [22-Ins_Local-Storage-Uh-oh/script.js](../../../../01-Class-Content/04-web-apis/01-Activities/22-Ins_Local-Storage-Uh-oh/script.js) in your IDE and point out the following: 

  * We are creating our object with the following code:

  ```js
  var user = {
    firstName: firstNameInput.value.trim(),
    lastName: lastNameInput.value.trim(),
    email: emailInput.value.trim(),
    password: passwordInput.value.trim()
  };
  ```
  * We are then setting this object into local storage.

  ```js
  localStorage.setItem("user", user);
  ```

* Ask the class the following question(s):

  * In what format does `localStorage` store data?
 
  * LocalStorage only stores string values.
 
  * What do you think we need to do to solve this problem? 

  * We need to convert our object to a string.

* Use student answers to transition to the next activity. 

### 10. Student Do: Local Storage With Objects (10 mins)

* Direct students to [23-Stu_Local-Storage-Objects/Unsolved/script.js](../../../../01-Class-Content/04-web-apis/01-Activities/23-Stu_Local-Storage-Objects/Unsolved/script.js).

```md
# Local Storage

* You have been provided with a sign up form that successfully submits user data and creates an object containing the data. Your job is to save this data to local storage and render the last submission to the page.

## Instructions

* Navigate to the MDN Docs on [JSON Stringify](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify). 

* Use the information there to convert your object to a string format.

* You will be working only inside of your `script.js` file.

* Inside your event listener modify `localStorage.setItem("user", user);` and `localStorage.getItem("user");` so they save and render the data.
```

### 11. Instructor Do: Review Local Storage With Objects (5 mins)

* Open [23-Stu_Local-Storage-Objects/Solved/script.js](../../../../01-Class-Content/04-web-apis/01-Activities/23-Stu_Local-Storage-Objects/Solved/script.js) and point out the following: 

 * We can use `JSON.stringify` to convert an object to a JSON string. We simply pass our object into the `JSON.stringify` method.

  ```js
  localStorage.setItem("user", JSON.stringify(user));
  ```

 * When we retrieve our data from `localStorage`, we use `JSON.parse`. This method parses our JSON string and converts it into an object.

 * We pass our call to `localStorage.getItem` into the `JSON.Parse` method, storing the value as a variable.

  ```js
  var lastUser = JSON.parse(localStorage.getItem("user"));
  ```

  * Now that the string has been converted back into an object, we can use simple dot notation to access the key/value pairs.

  ```js
  userFirstNameSpan.textContent = lastUser.firstName;
  userLastNameSpan.textContent = lastUser.lastName;
  userEmailSpan.textContent = lastUser.email;
  userPasswordSpan.textContent = lastUser.password;
  ```

* Ask the class, "What is JSON?"

  * JSON is JavaScript Object Notation. JSON is text, and any JavaScript object can be converted to JSON.

  * Because its text, we are able to store it into `localstorage` as the string that is required.

### 12. Instructor Do: Demo Data-Attributes (5 mins)

* Open [24-Ins_Data-Attributes/index.html](../../../../01-Class-Content/04-web-apis/01-Activities/24-Ins_Data-Attributes/index.html) in your browser and demo the functionality of the application:

  * When we click on a gif, their `state` changes from "animated" to "still".

* Open Chrome developer tools to demonstrate how the URLs are changing when we click.

* Open [24-Ins_Data-Attributes/script.js](../../../../01-Class-Content/04-Web-APIs/01-Activities/24-Ins_Data-Attributes/script.js) in your IDE and point out the followingL:

 * First we select our image container element.

  ```js
  var imageContainer = document.querySelector(".img-container");
  ```

 * We then listen for click on the container. 
 
  ```js
  imageContainer.addEventListener("click", function(event) {
    var element = event.target;
  ```

 * If an image is clicked, we get the `data-state` attribute value. 
 
    ```js
    if (element.matches("img")) {
      var state = element.getAttribute("data-state");
    ```

 * If the state is `still`, we then change it to `animate` and update the src url to an animated gif. 
 
  ```js
  if (state === "still") {
    element.setAttribute("data-state", "animate");
    element.setAttribute("src", element.getAttribute("data-animate"));
  ```

 * If the state is `animated`, we then change it to `still` and update the src attribute to a still image.

  ```js
      } else if (state === "animate") {
        element.setAttribute("data-state", "still");
        element.setAttribute("src", element.getAttribute("data-still"));
      }
    }
  });
  ```

* Ask the class, 'How can we access and update the different attributes of our application?'
 
  * With `getAttribute` and `setAttribute`.

* Answer any questions before proceeding to the next activity.

### 13. Students Do: Render Todos (10 mins)

* Direct students to the next activity, found in [25-Stu_Render-Todos/Unsolved](../../../../01-Class-Content/04-web-apis/01-Activities/25-Stu_Render-Todos/Unsolved).

```md
# Render Todos

In this activity you will be writing code to render an array of todo items to the list.

## Instructions

* Open the `script.js` file provided to you. You have been provided the necessary variable declarations as well as an array of todo items.

* Your goal is to create a function that will render our todos into a list in the browser.

  * Initially set the text content of the todoList to an empty string.
  
  * todoCountSpan should show the total count of todos on the page.
  
* Inside of your render function you will also need a for loop.

  * It should loop over the `todos` array creating an `li` element for each index of the array.
  
  * It should set the content of the created `li` element to the value of the current array index.
  
  * Finally the new `li` should be appended to the `ul` provided.
```

### 14. Instructor Do: Review Render Todos (5 mins)

* Open [25-Stu_Render-Todos/Solved](../../../../01-Class-Content/04-web-apis/01-Activities/25-Stu_Render-Todos/Solved) and point out the following: 

  * First we create our `renderTodos()` function. 

    ```js
    function renderTodos() {

      // ...
    }
    ```

  * Next inside our function we set the `innerHTML` of our `todoList` element to be a blank string. 

    ```js
    function renderTodos() {
      todoList.innerHTML = "";
    ```

  * We then set the `todoCountSpan` text content to the length of our `todos` array.
  
  ```js
    todoCountSpan.textContent = todos.length;
  }
  ```

  * Next we create a for loop that sets the value of the todos array at each index to a variable todo. 
  
  ```js
  for (var i = 0; i < todos.length; i++) {
    var todo = todos[i];
  ```
  
  * We then creates a new `li` element and set the `textContent` of the newly created `li` to the variable we created at the beginning of our loop. 
          
  ```js
  var li = document.createElement("li");
  li.textContent = todo;
  ```

  *Finally, we append our new list item to our existing `todoList` as a child.

  ```js
  todoList.appendChild(li);
  ```

  * Now that we have created a function that will render our todos, we can invoke it after our array variable declaration.

  ```js
  var todos = ["Learn HTML", "Learn CSS", "Learn JavaScript"];
  renderTodos();
  ```

* Answer any questions before proceeding to the next activity. 

- - -

### 15. Everyone Do: BREAK (30 mins)

- - - 

### 16. Students Do: Add Todos (10 mins)

* Direct students to the next activity, found in [26-Stu_Add-Todos/Unsolved](../../../../01-Class-Content/04-web-apis/01-Activities/26-Stu_Add-Todos/Unsolved)

```md
# Add ToDo's

In this activity, we will be continuing to build on our Todo activity. This time, we'll be adding the `add` functionality.

## Instructions

* Add an event listener so that when a user hits enter, the value from the todo input field is pushed to our todo array.

* Make sure that empty values are not pushed to the array.

* Once the value has been added to the array, clear the input field and re-render the todo list.
```

### 17. Instructor Do: Review Add Todos (5 mins)

* Open [26-Stu_Add-Todos/Solved/index.html](../../../../01-Class-Content/04-web-apis/01-Activities/26-Stu_Add-Todos/Solved/index.html) in your browser and briefly demonstrate the new functionality by adding new items to the todo list.

* Next navigate to [26-Stu_Add-Todos/Solved/script.js](../../../../01-Class-Content/04-web-apis/01-Activities/26-Stu_Add-Todos/Solved/script.js) in your IDE and point out the following:

  * We're listening for the `submit` event. Note that it would also be acceptable to add a keydown listener and check if the key pressed is enter. Mention that instead listening for `submit`, requires less code. Additionally, the callback function doesn't need to be ran needlessly every time the user presses a key.

  ```js
  todoForm.addEventListener("submit", function(event) {
  event.preventDefault();
  ```

  * 🗒 It's worth noting that there are two form behaviors that most commonly cause the `submit` event. The `return` key is pressed within a text input field or a submit button is clicked. This could be desired behavior, but remind students that there are situations where this behavior is not desired.

  * The `.trim()` method removes whitespace from before and after the input.

  ```js
  var todoText = todoInput.value.trim();
  ```

  * If the todoText is empty, return. This will prevent us from pushing empty strings to the todos array.

  ```js
  if (todoText === "") {
    return;
  }
  ```

  * Lastly, we push the `todoText` to the todos array, reset its value, and render the todos again.

  ```js
  todos.push(todoText);
  todoInput.value = "";

  renderTodos();
  });
  ```

* Answer any questions before moving to the next activity.

### 18. Students Do: Complete Todos (15 mins)

* Direct students towards the next activity located in [27-Stu_Complete-Todos/Unsolved](../../../../01-Class-Content/04-web-apis/01-Activities/27-Stu_Complete-Todos/Unsolved).

```md
# Complete Todos

In this activity, we will create a "complete" button that successfully removes a todo item from the list when clicked.

## Instructions

* Modify your `renderTodos()` function:

  * When a new todo is created, add a `data-index` for each `li`.

  * Generate a button that says "complete" and append it to your `li`.

* Add an event listener so that when a user clicks the complete button, it accesses the `data-index` value and removes that todo element from the list.

## Hint

* You can use `setAttribute` for `data-index` and `splice` to remove your todo from the list.
```

### 19. Instructor Do: Review Complete Todos (10 mins)

* Open [27-Stu_Complete-Todos/Solved/script.js](../../../../01-Class-Content/04-web-apis/01-Activities/27-Stu_Complete-Todos/Solved) in your IDE and point out the following:

  * First show students the unsolved `renderTodos()` function for context.

  ```js
  function renderTodos() {

    todoList.innerHTML = "";
    todoCountSpan.textContent = todos.length;

    for (var i = 0; i < todos.length; i++) {
      var todo = todos[i];

      var li = document.createElement("li");
      li.textContent = todo;
      todoList.appendChild(li);
    }
  }
  ```

  * In order to track which todo we are going to mark as complete, we need set a `data-index` that points to each todo's index, `i` from our for loop.

  ```js
  li.setAttribute("data-index", i);
  ```

  * Next we can create a "Complete" button for each item and set it's text to "Complete".

  ```js
  var button = document.createElement("button");
      button.textContent = "Complete";
  ```

  * Finally we append our newly updated button to our `li`.

  ```js
  li.appendChild(button);
  ```

  * Now that our todo can be marked as complete, we have to create an event listener for our button that removes it from the list when clicked.

  ```js
  todoList.addEventListener("click", function(event) {
  ```
  * Next we set a variable for `event.target`. When an element is clicked, we check if it was a button and if so, grab the `data-index` of that element.

  ```js
  var element = event.target;
  ```

  * We then use `.splice` to remove the element with that index and rerender or todos by calling `renderTodos()`. The `.splice` method allows us to change the contents of an array. We can use it to remove, replace, or add new elements. 

  ```js
  if (element.matches("button") === true) {
    var index = element.parentElement.getAttribute("data-index");
    todos.splice(index, 1);
    renderTodos();
  ```

* Check for understanding and answer any lingering questions before moving on.

### 20. Students Do: Local Storage Todos (10 mins)

* Direct students towards their next activity located in [28-Stu_Local-Storage-Todos/Unsolved](../../../../01-Class-Content/04-web-apis/01-Activities/28-Stu_Local-Storage-Todos/Unsolved).

  ```md
  # Local Storage Todo's

  In this activity, we will work on storing our todos in `localStorage`. 

  ## Instructions

  * Inside the `init()` function:

    * Set a variable called `storedTodos` that retrieves the todos from `localStorage` and parses the JSON string to an object.

    * Check if the todos were retrieved from `localStorage` and if so, set a `todos` variable with the `storedTodos`.

    * Lastly, render the todos to the DOM.

  * Inside the `storeTodos()` function:

    * Stringify and set the "todos" key in `localStorage` to the `todos` array.

  ## Hint

  * You will need to use `JSON.stringify` and `JSON.parse`.
  ```

### 21. Instructor Do: Review Local Storage Todos (5 mins)

* Open [28-Stu_Local-Storage-Todos/Solved/script.js](../../../../01-Class-Content/04-web-apis/01-Activities/28-Stu_Local-Storage-Todos/Solved/script.js) in your IDE and walk students through the solved code.

* Inside the `init()` function we set a variable called `storedTodos` that retrieves the todos from `localStorage` and parses the JSON string to an object.

  ```js
  var storedTodos = JSON.parse(localStorage.getItem("todos"));
  ```

* We then check if the todos were retrieved from `localStorage` and if so, set a `todos` variable with the `storedTodos` and the render the todos to the DOM.

  ```js
  if (storedTodos !== null) {
      todos = storedTodos;
    }

  renderTodos();
  ```

* Inside our `storeTodos()` function we stringify and set the "todos" key in `localStorage` to the `todos` array.

  ```js
  localStorage.setItem("todos", JSON.stringify(todos));
  ```

* Answer any questions before letting students out for break.

### 22. Students Do: Timer App (60 mins)

* Direct students to their final activity of the day located in [29-Stu_Timer-App/Unsolved](../../../../01-Class-Content/04-web-apis/01-Activities/29-Stu_Timer-App/Unsolved)

```md
# Tomato Timer

## Instructions

* In this activity, we will be creating a "tomato" timer that allows the user to set a timer with working and resting periods. We will also store the length of each period in local storage so that the user's preferences persist, even if the browser is closed.

* You have been provided with all of the HTML and CSS that you'll need. 

* Begin by opening `index.html` in your browser. Take a moment to identify different elements on the page that will need functionality:

  * Time left display

  * Start button

  * Pause button

  * Stop button

  * Status toggle

* **Part One** Create functions in `script.js` to add support for the following features:

  1. Create a function that initializes the timer by taking the minutes input from the user and setting the `tototalSeconds` variable. Since we'll be using this function to reset as well, clear any existing intervals.

  2. When the timer starts, update the DOM every second to reflect the time left. It is recommended that you create separate functions to properly format the minutes and seconds.

  3. When the timer is finished, alert the user that it is time to take a break.

* **Part Two**: Add functionality to the pause and stop buttons.

  1. The pause button should temporarily stop the timer. This means that if play is pressed again, the timer will continue where it left off.

  2. The stop button should reset the timer. If play is pressed again, the timer should start over.

* **Part Three**: Add the ability to switch back and forth between working time and resting time.

  1. Set up a variable to keep track of which mode the timer is in.

  2. If the timer is in working mode, then it should alert the user "Time for a break!" upon completion.

  3. If the timer is in resting mode, it should alert the user "Time to get back to work!" upon completion.

  4. Whenever the switch is clicked, the DOM should update with the current status, and the timer should reset.

  5. Make sure that the timer is using minutes of work in work mode and minutes of rest, respectively. 

* **Part Four**: Add localStorage to the application

  1. Every time the user starts a timer, the minutes of work and minutes of rest should be saved to localStorage.

  2. Upon page load, the minutes of work and minutes of rest input fields should be initialized to their previously stored values.
```

### 23. Instructor Do: Review Timer App (10 mins)

* Open [29-Stu_Timer-App/Solved](../../../../01-Class-Content/04-web-apis/01-Activities/29-Stu_Timer-App/Solved/script.js) in your IDE and point out the following key aspects:

  * We create a function called `starTimer`. We will use it to call our `setTime` function.
  
  ```js
  function startTimer() {
    setTime();
  ```
  
  * We create a `setInterval` function and store it inside a variable called interval.

  ```js
    interval = setInterval(function() {
      secondsElapsed++;
      renderTime();
    }, 1000);
  }
  ```

  * Ask the class, "Why do we store our setInterval into a variable?"

  * So we can clear it later.

  * We create a function call `pauseTimer` which will clear our interval and call our `renderTime` function.

  ```js
  function pauseTimer() {
    clearInterval(interval);
    renderTime();
  }
  ```

  * We create a function called `stopTimer`. When this function is invoked we set our secondsElapsed to `0`, and call our `setTime` and `renderTime` functions.

  ```js
  function stopTimer() {
    secondsElapsed = 0;
    setTime();
    renderTime();
  }
  ```

  * We create a `setTimePreferences` function. We use `localStorage.setItem`, naming the key `preferences` and stringifying our work minutes and rest minutes so they can be added into localstorage.

  ```js
  function setTimePreferences() {
    localStorage.setItem(
      "preferences",
      JSON.stringify({
        workMinutes: workMinutesInput.value.trim(),
        restMinutes: restMinutesInput.value.trim()
      })
    );
   ```

  * We create a `getTimePreferences` function. Next we `JSON.Parse` the getting of our `preferences` localstorage item to turn our stringified object back into an object

  ```js
  function getTimePreferences() {
    var preferences = JSON.parse(localStorage.getItem("preferences"));
  ```

* If preferences exists in localstorage and has a key/value pair of `workMinutes` we set our `workMinutesInput.value` to be equal to our preference objects `workMinutes` value.

  ```js
  if (preferences) {
    if (preferences.workMinutes) {
      workMinutesInput.value = preferences.workMinutes;
    }
  ```
  
  * If preferences instead has no `workMinutes` but has `restMinutes` then we set our `restMinutesInput.value` to be equal to our preference objects `restMinutes` value.

  ```js
    if (preferences.restMinutes) {
      restMinutesInput.value = preferences.restMinutes;
    }
  }
  ```

* Answer any remaining questions students may have and end class for the day.

### 24. END (0 mins)

- - -

## Lesson Plan Feedback

How did today’s lesson go? Your feedback is important. Please take 5 minutes to complete this anonymous survey.

[Class Survey](https://forms.gle/nYLbt6NZUNJMJ1h38)
