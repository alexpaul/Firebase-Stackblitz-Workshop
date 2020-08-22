# Firebase-Stackblitz-Workshop

Firebase / Stackblitz workshop.

Workshop led by Frank va Pullelen. We built two web apps during the workshop. 

## Weather app

#### HTML 

```html 
<label>Loading weather...</label>
<br><br>
<button>Sunny</button>
<button>Foggy</button>
<button>Smoky</button>
```

![weather app](Assets/weatherapp.png)

#### JavaScript 

```javasctipt 
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

## Chat app

#### HTML 

```html 
<ol>Loading messages...</ol>

<form>
  <input>
  <input type='submit'>
</form>
```

![chat app](Assets/chatapp.png)

#### JavaScript 

```javasctipt 
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

## Resources 

1. [Stackblitz - online web IDE](https://stackblitz.com)
2. [Firebase Console - Here you create and manage your Firebase projects](https://console.firebase.google.com/u/0/)
3. [Frank va Pullelen @puf on Twitter](https://twitter.com/puf)
