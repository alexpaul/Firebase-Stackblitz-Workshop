# Firebase Stackblitz Workshop

Workshop led by [Frank van Pullelen](https://twitter.com/puf). We built two web apps during the workshop. The web apps were built using [Stackblitz](https://stackblitz.com) and the backend is done using Firebase.

## Weather app

In the weather app when the user selects a weather button firestore database gets updated. 

The Realtime database was used for the weather app. 

#### Web app

![weather app](Assets/weatherapp.png)

#### HTML 

```html 
<label>Loading weather...</label>
<br><br>
<button>Sunny</button>
<button>Foggy</button>
<button>Smoky</button>
```

#### JavaScript 

```javascript
// Import stylesheets
import './style.css';

import firebase from 'firebase'; 

firebase.initializeApp({
  databaseURL: 'https://weathersf-fb38e.firebaseio.com/'
});

let ref = firebase.database().ref('weather'); 

let buttons = document.querySelectorAll("button"); 
buttons.forEach((button) => {
  button.addEventListener("click", (e) => {
    //document.querySelector("label").textContent = e.target.textContent;
    ref.set(e.target.textContent); 
  });
});

ref.on("value", (snapshot) => {
  document.querySelector("label").textContent = snapshot.val(); 
});
```

#### Firebase Realtime Schema

Create a Firebase project `weatherapp` via your Firebase console. 

![realtime](Assets/weatherapp-realtime.png)

#### Realtime Database Rules 

The Firebase Security Rules below makes sure that the new data being sent to the database is a `String` and that it matches either `Sunny` or `Foggy` (San Francisco weather ☀️ 🌁) using Regular expressions. 

```javascript
{
  "rules": {
    "weather": {
      ".read": true,
      ".write": true,
      ".validate": "newData.isString() && newData.val().matches(/^(Sunny|Foggy)/)" 
    }    
  }
}
```

#### [weather app](https://js-weathersf.stackblitz.io)


## Chat app

In this chat app when the user adds a new chat message and presses the "submit" button it gets sent to Firestore database and also renders in the web app. 

Firebase Firestore was used for the chat app. Firestore has better offline support. More scaling support for a a huge number of users. 

#### HTML 

```html 
<ol>Loading messages...</ol>

<form>
  <input>
  <input type='submit'>
</form>
```

#### Web app

![chat app](Assets/chatapp.png)

#### JavaScript 

```javascript 
// Import stylesheets
import './style.css';

import firebase from 'firebase'; 

var firebaseConfig = {
    apiKey: "AIzaSyBu6qqqdYImuOQ6SYeu3p93eh1AFaooLhg",
    authDomain: "fireconf-2018-78d86.firebaseapp.com",
    databaseURL: "https://fireconf-2018-78d86.firebaseio.com",
    projectId: "fireconf-2018-78d86",
    storageBucket: "fireconf-2018-78d86.appspot.com",
    messagingSenderId: "857622937888",
    appId: "1:857622937888:web:cc37b0df15d2283bc1d745",
    measurementId: "G-RFXM3RC0ZJ"
  };

// Initialize Firebase  
firebase.initializeApp(firebaseConfig); 

// // ref to chat documents 
let chat = firebase.firestore().collection("chat");

document.querySelector("form").addEventListener("submit", (e) => {
  e.preventDefault(); 

  let message = document.querySelector("input").value; 

  chat.add({
    message: message, 
    timestamp: Date.now() 
  }); 

  document.querySelector("input").value = ""; 

  return false
});

chat.orderBy("timestamp", "desc").limit(10).onSnapshot((querySnatshot) => {
  let list = document.querySelector("ol"); 
  querySnatshot.forEach((doc) => {
    let li = document.createElement("li"); 
    li.textContent = doc.data().message; 
    list.append(li); 
  }); 
});
```

#### Firebase Firestore Schema

Create a Firebase project `chatapp` via your Firebase console. 

![firestore](Assets/chatapp-firestore.png)

#### [chatapp](https://js-chatapp.stackblitz.io/)


## Resources 

1. [Stackblitz - online web IDE](https://stackblitz.com)
2. [Firebase Console - Here you create and manage your Firebase projects](https://console.firebase.google.com/u/0/)
3. [Frank va Pullelen @puf on Twitter](https://twitter.com/puf)
4. [Meetup Event](https://www.meetup.com/gdgnyc/events/272257853)
