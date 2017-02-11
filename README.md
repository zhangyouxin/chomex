chomex
========

[![Build Status](https://travis-ci.org/otiai10/chomex.svg?branch=master)](https://travis-ci.org/otiai10/chomex) [![Dependency Status](https://gemnasium.com/badges/github.com/otiai10/chomex.svg)](https://gemnasium.com/github.com/otiai10/chomex)

Chrome Extension Minimum Routing Kit.

would look like these

- [otiai10/kanColleWidget: MessageRoutes.js](https://github.com/otiai10/kanColleWidget/blob/develop/src/js/Components/Routes/MessageRoutes.js)

# Features

- Router
  - Controllers should be the same interface defined by Chrome
- Model
  - is simple accessor for `localStorage` like ActiveRecord

# Example

## `background.js` like as a server

```javascript
import { Router } from 'chomex';

let r1 = new Router();
r1.on('/hello', (message) => {
  return {data: `hello, ${message.user}!`};
});

chrome.runtime.onMessage.addListener(r1.listener());
```

Customize resolve rule, so that you can use other type of EventListener.

```javascript
const resolve = (notificationId) => {
  [ , name] = notificationId.match(/([a-z]+)\.[a-z]+/);
  return {name};
};
let r2 = new Router(resolve);

// This route will fire if notificationId == "xxx.foobar"
r2.on('xxx', (notificationId) => {
  alert('clicked!')
});

chrome.notifications.onClicked.addListener(r2.listener());
```

## `content_script.js` as a client

```javascript
// content_script.js
// just like a client
import { Client } from 'chomex';

const client = new Client(chrome.runtime);

client.message('/hello', {user: 'otiai10'}).then((res) => {
  console.log(res);
});
// OUTPUT:
// {status:200, data: 'hello, otiai10!'}
```

```javascript
chorme.notifications.create('xxx.otiai20', body);
// When this notification clicked,
// Background will alert "clicked!!"
```
