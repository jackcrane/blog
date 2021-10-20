---
layout: post
title:  "Github readme Tik Tak Toe: Technical Discussion"
date:   2021-10-19 16:32:00 -0400
image:
  path: https://og-image.xyz/og/GH Tik Tak Toe/Technical Discussion/blog.jackcrane.rocks/https/menlo/cheerfulorange/{{h}}ffffff/data.png
---

## Github readme tik-tak-toe

Powered by Cloudflare Workers, this is meant to be a tik-tak-toe game visitors to my GitHub Profile can play. Due to the limitations of GH being frontend, it works by requesting an image for that particular cell in a 3x3 table wrapped in a link that is also affiliated with the cell on the backend. This allows the game to be played from static sites around the internet. Unfortunately, GitHub's caching service breaks the images, even though I have been working on fixing caching issues on the backend.

I know what I am trying to do with dynamic images is possible because of the existance of badges on GitHub readmes that often have to change quickly. I have reached out to GH support and am waiting to hear back.

**How I made this and the full code is at the bottom :)**

[Play it live on GitHub](https://github.com/jackcrane) (or on the grid below)

[View the source code](https://github.com/jackcrane/gh-tik-tak-toe)

---

## The game

Current player: 

<img src="https://gh-tik-tak-toe.jackcrane.workers.dev/current-player?escape-cache">

<table>
  <tr>
    <td>
      <a href="https://gh-tik-tak-toe.jackcrane.workers.dev/move/0">
        <img src="https://gh-tik-tak-toe.jackcrane.workers.dev/image/0?escape-cache">
      </a>
    </td>
    <td>
      <a href="https://gh-tik-tak-toe.jackcrane.workers.dev/move/1">
        <img src="https://gh-tik-tak-toe.jackcrane.workers.dev/image/1?escape-cache">
      </a>
    </td>
    <td>
      <a href="https://gh-tik-tak-toe.jackcrane.workers.dev/move/2">
        <img src="https://gh-tik-tak-toe.jackcrane.workers.dev/image/2?escape-cache">
      </a>
    </td>
  </tr>
  <tr>
    <td>
      <a href="https://gh-tik-tak-toe.jackcrane.workers.dev/move/3">
        <img src="https://gh-tik-tak-toe.jackcrane.workers.dev/image/3?escape-cache">
      </a>
    </td>
    <td>
      <a href="https://gh-tik-tak-toe.jackcrane.workers.dev/move/4">
        <img src="https://gh-tik-tak-toe.jackcrane.workers.dev/image/4?escape-cache">
      </a>
    </td>
    <td>
      <a href="https://gh-tik-tak-toe.jackcrane.workers.dev/move/5">
        <img src="https://gh-tik-tak-toe.jackcrane.workers.dev/image/5?escape-cache">
      </a>
    </td>
  </tr>
  <tr>
    <td>
      <a href="https://gh-tik-tak-toe.jackcrane.workers.dev/move/6">
        <img src="https://gh-tik-tak-toe.jackcrane.workers.dev/image/6?escape-cache">
      </a>
    </td>
    <td>
      <a href="https://gh-tik-tak-toe.jackcrane.workers.dev/move/7">
        <img src="https://gh-tik-tak-toe.jackcrane.workers.dev/image/7?escape-cache">
      </a>
    </td>
    <td>
      <a href="https://gh-tik-tak-toe.jackcrane.workers.dev/move/8">
        <img src="https://gh-tik-tak-toe.jackcrane.workers.dev/image/8?escape-cache">
      </a>
    </td>
  </tr>
</table>

(You will need to reload the page after you make a move, otherwise new images will not be fetched)

[Reset Game](https://gh-tik-tak-toe.jackcrane.workers.dev/reset)

---

## Wow cool! How does it work?

### First, the tech stack:

- Cloudflare Workers
- Cloudflare Workers KV
- GitHub `jackcrane/jackcrane` repo readme.md

### The constraints

Due to the whole idea of GitHub readmes, there is no backend or javascript support, which is what makes this otherwise trivial game a fun project to hack at. 

### The idea & frontend

I started by making a 3x3 table in my GH readme (and on this blog post) with a nested link and nested image. Because of all the nesting, I found I needed to use HTML over markdown for the table element:

```html
<table>
  <tr>
    <td>
      <a href="https://gh-tik-tak-toe.jackcrane.workers.dev/move/0">
        <img src="https://gh-tik-tak-toe.jackcrane.workers.dev/image/0">
      </a>
    </td>
    <td>
      <a href="https://gh-tik-tak-toe.jackcrane.workers.dev/move/1">
        <img src="https://gh-tik-tak-toe.jackcrane.workers.dev/image/1">
      </a>
    </td>
    <td>
      <a href="https://gh-tik-tak-toe.jackcrane.workers.dev/move/2">
        <img src="https://gh-tik-tak-toe.jackcrane.workers.dev/image/2">
      </a>
    </td>
  </tr>
  ... (2 more tr's)
</table>
```

Lets break down each of those links. If you have even a foundational understanding of HTML, you will recognize the `a` tag as a link and the `img` tag as an image. Each of the `a` tags are used to handle clicks and the `img`s are used for rendering game states. Notice how both the link and image in each 'cell' are the same between eachother but unique against the other cells in the table. This is how I am handling clicks, passed in the route.

Once rendered, the table will look like: (each number is the reference to the cell in our code)

|0|1|2|
|3|4|5|
|6|7|8|

### The logic & backend

The backend is broken into 5 routes:
- `move/:cell`
- `image/:cell`
- `debug`
- `reset`
- `current-player`

The `move` route handles all of the actual logic. It is where each the links lead and it handles executing the move, determining if there is a winner, and saving states to Cloudflare's Workers KV database engine.

The `image` route simply returns the image of the piece (blank, circle, or x) in the given cell.

The `debug` route just returns the game board as JSON

The `reset` route sets the states in the KV database to an empty board and resets everything in the backend

The `current-player` route returns the image of the current player (circle or x) for use in the 'current player' interface.

### Starting the code

#### Default code

Running `wrangler init` results in the following code in `index.js`:

```javascript
addEventListener("fetch", event => {
  event.respondWith(handleRequest(event.request))
})

async function handleRequest(request) {
  return new Response("Hello worker!", {
    headers: { "content-type": "text/plain" }
  })
}
```

#### Helpers

And at the top lets add all our helper functions and objects:

```javascript
let table = [
  0, 0, 0, 0, 0, 0, 0, 0, 0
]

const winningConditions = [
  [0, 1, 2],
  [3, 4, 5],
  [6, 7, 8],
  [0, 3, 6],
  [1, 4, 7],
  [2, 5, 8],
  [0, 4, 8],
  [2, 4, 6]
];

// https://stackoverflow.com/a/1349426/9503170
const makeid = (length=64) => {
  let result = '';
  let characters = 'ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789';
  let charactersLength = characters.length;
  for (let i = 0; i < length; i++) {
    result += characters.charAt(Math.floor(Math.random() * charactersLength));
  }
  return result;
}

let gameRunning = true;
let gameWon = false;

addEventListener("fetch", event => {
...
```

And now for explaining:
- `table` is a variable we will be using to hold the state of the game. Remember serverless functions are (by nature) ephemeral so we don't want to waste time and and effort fetching an immutable game state from the database, so we fetch it once and drop it in this array.
- `winningConditions` is a 2-dimensional array holing all of the possible ways to win a tik-tak-toe game. We use this array to determine if a player has won the game.
- `makeid` is a random string generator we use to generate a random alphanumeric string that we use to tell some services' caches that this is a new image and it should override whatever they have cached.
- `gameWon` holds if a game has been won. It is used several times determining if we need to execute logic or what we need to return to the users.

#### The `registerMove` function

```javascript
const registerMove = async (spot, activePlayer) => {
  let gs = await GAMESTORE.get('gamestate')
  table = await JSON.parse(gs)
  if(table[spot] == 0) {  
    table[spot] = activePlayer;
    for(let i = 0; i < winningConditions.length; i++) {
      if(
        (
          (
            table[winningConditions[i][0]] == 
            table[winningConditions[i][1]]
          ) && (
            table[winningConditions[i][1]] ==
            table[winningConditions[i][2]]
          ) && (
            table[winningConditions[i][0]] ==
            table[winningConditions[i][2]]
          )
        ) && (
          table[winningConditions[i][0]] != 0 &&
          table[winningConditions[i][1]] != 0 &&
          table[winningConditions[i][2]] != 0
        )
      ) {
        gameWon = true;
      }
    }
    if(activePlayer === 1) {
      await GAMESTORE.put('player', '2');
    } else if(activePlayer === 2) {
      await GAMESTORE.put('player', '1');
    }
  }

  await GAMESTORE.put('gamestate', JSON.stringify(table))
  return true;
}
```

Please ignore the absurd if statement formatting, but it was the only way I could get it to look that didnt give me a headache.

We start by declaring an async function called `registerMove` that takes a `spot` and `activePlayer` argument. The `spot` is the cell the player chose, and the `activePlayer` is the active player (surprising, right).

We set the `gs` variable to the data we pull from the Cloudflare KV database we have set up with the name `GAMESTORE`. Keep in mind this data is JSON, so we need to parse it before use, which we do and update the variable `table` with it.

In our if statement, we need to be sure the cell is not already taken so we dont overwrite the value.

Then we set the cell in question to the activePlayer (1 or 2). 

Next we have to iterate over the `winningConditions` array, trying to determine if the match has been won. The logic is as follows:

`winningConditions[i] = [0, 1, 2]`. We need to make sure all values in `winningConditions[i]` match our 2 test cases:
1. They are not `0`
2. They are all the same

If they pass our test cases, we know the game has been won, so we update our `gameWon` variable accordingly.

The final action in our if statement is to switch the active player, switching the number and saving it to the KV database.

Finally, we save the game state (saved in the `table` variable) to our KV database, then return `true`.

#### Update the `handleRequest` function

We need to update the `handleRequest` function to handle simple routing and function calls. For readability purposes I converted handleRequest to an arrow function, but this is not necessary.

```javascript
const handleRequest = async(request) => {
  if(typeof(await GAMESTORE.get('gamestate')) === 'null') {
    await GAMESTORE.put('gamestate', '[0,0,0,0,0,0,0,0,0]')
  }
  if(typeof(await GAMESTORE.get('player')) === 'null') {
    await GAMESTORE.put('player', '1')
  }

  table = JSON.parse(await GAMESTORE.get('gamestate'))

  if(request.url.includes('/move/')) {
    // Move route goes here
  } else if(request.url.includes('/image/')) {
    // Image route goes here
  } else if(request.url.includes('debug')) {
    // Debug route goes here
  } else if(request.url.includes('reset')) {
    // Reset route goes here
  } else if(request.url.includes('current-player')) {
    // Current player route goes here
  } else {
    return new Response('Something went wrong', {
      headers: { 'content-type': 'text/plain' }
    })
  }
}
```

The `handleRequest` function is what accepts the request and breaks it into routes. 

It starts by verifying the presence of the necessary keys in the KV database, and if they are not present it creates them. Then it gets the `gamestate` from the db and saves it to the `table` variable.

Next the function breaks the requests in to routes. For simplicity sake, I chose not to use a router script but rather to implement my own extremely basic router simply based on text included in strings.

#### The `move` route

Insert the following code at the comment `Move route goes here` comment above.

```javascript
...
if(request.url.includes('/move/')) {
  let path = request.url.split('/move/')[1]?.split('?')[0]
  let player = parseInt(await GAMESTORE.get('player'));

  if(await registerMove(path, player)) {
    if(gameWon) {
      await GAMESTORE.put('gamestate', '[0,0,0,0,0,0,0,0,0]')
      await GAMESTORE.put('player', '1')
      gameWon = false;
      return new Response('Player has won', {
        headers: { 'content-type': 'text/plain' },
      })
    } else {
      return new Response('Play successfully registered', {
        headers: { 'content-type': 'text/plain' },
      })
    }
  } else {
    await GAMESTORE.put('gamestate', '[0,0,0,0,0,0,0,0,0]')
    await GAMESTORE.put('player', '1')
    gameWon = false;
    return new Response('Player has won', {
      headers: { 'content-type': 'text/plain' },
    })
  };
} else if(request.url.includes('/image/')) {
...
```

The `move` route is what handles the logic of the game. We start by getting the parameter out of the url (we know it is after `/move/` and is before the `?` potentially used in debugging). Then we fetch the active player from the KV database.

We call the `registerMove` function which returns `true` if there are no errors, and will return `null` if there are any errors or someone won. Next we check to see if the player won several times, and if the game has been won, we reset it by setting the `player` back to `1` and by setting the `gamestate` back to `[0,0,0,0,0,0,0,0,0]`. If the match has been won, we return `Player has won` to the client, and otherwise we return `Play successfully registered`.

#### The `image` route

```javascript
...
} else if(request.url.includes('/image/')) {
  let blob;
  let blobBlank = await fetch('https://img.icons8.com/ios/100/000000/unchecked-checkbox.png').then(r => r.blob());
  let blobXed = await fetch('https://img.icons8.com/fluency-systems-regular/96/000000/x.png').then(r => r.blob());
  let blobCircle = await fetch('https://img.icons8.com/ios/100/000000/circled.png').then(r => r.blob());
  let blobError = await fetch('https://img.icons8.com/ios/50/000000/error--v1.png').then(r => r.blob());


  let path = parseInt(request.url.split('/image/')[1]?.split('?')[0]);

  switch(table[path]) {
    case 0:
      blob = blobBlank
      break;
    case 1:
      blob = blobXed
      break;
    case 2:
      blob = blobCircle
      break;
    default:
      blob = blobError;
      break;
  }

  return new Response(blob, {
    headers: { 
      'content-type': 'image/png',
      'Cache-Control': 'no-cache',
      'ETag': makeid(64)
    }
  })
} else if(request.url.includes('debug')) {
...
```

We start the `image` route by setting a functionally-scoped variable called `blob` (note this is left undefined). We also set `blobBlank`, `blobXed`, `blobCircle`, and `blobError` to PNG images we have fetched then turned to ReadableStreams using the `blob()` functions.

Then we get the path just like we did in the `move` route. This path is the cell that holds the image we are returning in our frontend. We drop that into a switch for the 3 possible states of the table in that given position, 0, 1, or 2. If it is a 0, we set the functionally-scoped `blob` variable to the variable holding the blobified blank image (actually more of a round square because I couldn't find a blank one but it gets the job done). If it is 1, we set it to the variable holding the X image, and if it is a 2, we set it to the variable holding the circle image.

Finally, we return the blob of the image as well as the headers for a `content-type` of `image/png`, `Cache-Control` of `no-cache` and `ETag` of the 64-character long random string returned by the `makeid()` function. The `Cache-Control` and the `ETag` headers are critical to keep GitHub or other providers from caching your images for an eternity.

#### The `debug` route

```javascript
...
} else if(request.url.includes('debug')) {
  let v = await GAMESTORE.get('gamestate');
  return new Response(v, { headers: { 'content-type': 'text/plain' }})
} else if(request.url.includes('reset')) {
...
```

The debug route is not actually necessary and is hopefully pretty self-explanatory at this point, so I wont explain it.

#### The `reset` route

```javascript
...
} else if(request.url.includes('reset')) {
  await GAMESTORE.put('gamestate', '[0,0,0,0,0,0,0,0,0]')
  await GAMESTORE.put('player', '1')
  return new Response('Game reset', {
    headers: { 'content-type': 'text/plain' }
  })
} else if(request.url.includes('current-player')) {
...
```

The reset route implements much of the same logic for when a player has won the game. We put an array of 9 zeros and reset the player to 1.

#### The current-player route

```javascript
...
} else if(request.url.includes('current-player')) {
  let blob;
  let blobBlank = await fetch('https://img.icons8.com/ios/100/000000/unchecked-checkbox.png').then(r => r.blob());
  let blobXed = await fetch('https://img.icons8.com/fluency-systems-regular/96/000000/x.png').then(r => r.blob());
  let blobCircle = await fetch('https://img.icons8.com/ios/100/000000/circled.png').then(r => r.blob());
  let blobError = await fetch('https://img.icons8.com/ios/50/000000/error--v1.png').then(r => r.blob());

  let player = await GAMESTORE.get('player');
  console.log(player)

  switch(parseInt(player)) {
    case 0:
      blob = blobBlank
      break;
    case 1:
      blob = blobXed
      break;
    case 2:
      blob = blobCircle
      break;
    default:
      blob = blobError;
      break;
  }
  return new Response(blob, {
    headers: { 
      'content-type': 'image/png',
      'Cache-Control': 'no-cache',
      'ETag': makeid(64)
    }
  })
} else {
...
```

You will notice the `current-player` route implements almost all the same logic as the `image` route, just instead of using the value of the path in the table, we are using the `player` variable pulled from the database.

## The full code:

```javascript
let table = [
  0, 0, 0, 0, 0, 0, 0, 0, 0
]

const winningConditions = [
  [0, 1, 2],
  [3, 4, 5],
  [6, 7, 8],
  [0, 3, 6],
  [1, 4, 7],
  [2, 5, 8],
  [0, 4, 8],
  [2, 4, 6]
];

const printTable = () => {
  console.log(table[0], table[1], table[2])
  console.log(table[3], table[4], table[5])
  console.log(table[6], table[7], table[8])
}

const makeid = (length=64) => {
  let result = '';
  let characters = 'ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789';
  let charactersLength = characters.length;
  for (let i = 0; i < length; i++) {
    result += characters.charAt(Math.floor(Math.random() * charactersLength));
  }
  return result;
}

let gameWon = false;

const registerMove = async (spot, position) => {
  let gs = await GAMESTORE.get('gamestate')
  table = await JSON.parse(gs)
  if(table[spot] == 0) {  
    table[spot] = position;
    for(let i = 0; i < winningConditions.length; i++) {
      if(
        (
          (
            table[winningConditions[i][0]] == 
            table[winningConditions[i][1]]
          ) && (
            table[winningConditions[i][1]] ==
            table[winningConditions[i][2]]
          ) && (
            table[winningConditions[i][0]] ==
            table[winningConditions[i][2]]
          )
        ) && (
          table[winningConditions[i][0]] != 0 &&
          table[winningConditions[i][1]] != 0 &&
          table[winningConditions[i][2]] != 0
        )
      ) {
        gameWon = true;
      }
    }
    if(position === 1) {
      await GAMESTORE.put('player', '2');
    } else if(position === 2) {
      await GAMESTORE.put('player', '1');
    }
  }

  await GAMESTORE.put('gamestate', JSON.stringify(table))
  return true;
}

addEventListener('fetch', event => {
  event.respondWith(handleRequest(event.request))
})

const handleRequest = async(request) => {
  if(typeof(await GAMESTORE.get('gamestate')) === 'null') {
    await GAMESTORE.put('gamestate', '[0,0,0,0,0,0,0,0,0]')
  }
  if(typeof(await GAMESTORE.get('player')) === 'null') {
    await GAMESTORE.put('player', '1')
  }

  table = JSON.parse(await GAMESTORE.get('gamestate'))

  if(request.url.includes('/move/')) {
    let path = request.url.split('/move/')[1]?.split('?')[0]
    let player = parseInt(await GAMESTORE.get('player'));
    if(await registerMove(path, player)) {
      if(gameWon) {
        await GAMESTORE.put('gamestate', '[0,0,0,0,0,0,0,0,0]')
        await GAMESTORE.put('player', '1')
        gameWon = false;
        return new Response('Player has won', {
          headers: { 'content-type': 'text/plain' },
        })
      } else {
        return new Response('Play successfully registered', {
          headers: { 'content-type': 'text/plain' },
        })
      }
    } else {
      await GAMESTORE.put('gamestate', '[0,0,0,0,0,0,0,0,0]')
      await GAMESTORE.put('player', '1')
      gameWon = false;
      return new Response('Player has won', {
        headers: { 'content-type': 'text/plain' },
      })
    };
  } else if(request.url.includes('/image/')) {

    let blob;
    let blobBlank = await fetch('https://img.icons8.com/ios/100/000000/unchecked-checkbox.png').then(r => r.blob());
    let blobXed = await fetch('https://img.icons8.com/fluency-systems-regular/96/000000/x.png').then(r => r.blob());
    let blobCircle = await fetch('https://img.icons8.com/ios/100/000000/circled.png').then(r => r.blob());
    let blobError = await fetch('https://img.icons8.com/ios/50/000000/error--v1.png').then(r => r.blob());
    

    let path = parseInt(request.url.split('/image/')[1]?.split('?')[0]);

    console.log('pos', path, table[path])

    switch(table[path]) {
      case 0:
        blob = blobBlank
        break;
      case 1:
        blob = blobXed
        break;
      case 2:
        blob = blobCircle
        break;
      default:
        blob = blobError;
        break;
    }

    return new Response(blob, {
      headers: { 
        'content-type': 'image/png',
        'Cache-Control': 'no-cache',
        'ETag': makeid(64)
      }
    })
  } else if(request.url.includes('debug')) {
    let v = await GAMESTORE.get('gamestate');
    return new Response(v, { headers: { 'content-type': 'text/plain' }})
  } else if(request.url.includes('reset')) {
    await GAMESTORE.put('gamestate', '[0,0,0,0,0,0,0,0,0]')
    await GAMESTORE.put('player', '1')
    return new Response('Game reset', {
      headers: { 'content-type': 'text/plain' }
    })
  } else if(request.url.includes('current-player')) {
    let blob;
    let blobBlank = await fetch('https://img.icons8.com/ios/100/000000/unchecked-checkbox.png').then(r => r.blob());
    let blobXed = await fetch('https://img.icons8.com/fluency-systems-regular/96/000000/x.png').then(r => r.blob());
    let blobCircle = await fetch('https://img.icons8.com/ios/100/000000/circled.png').then(r => r.blob());
    let blobError = await fetch('https://img.icons8.com/ios/50/000000/error--v1.png').then(r => r.blob());

    let player = await GAMESTORE.get('player');
    console.log(player)

    switch(parseInt(player)) {
      case 0:
        blob = blobBlank
        break;
      case 1:
        blob = blobXed
        break;
      case 2:
        blob = blobCircle
        break;
      default:
        blob = blobError;
        break;
    }
    return new Response(blob, {
      headers: { 
        'content-type': 'image/png',
        'Cache-Control': 'no-cache',
        'ETag': makeid(64)
      }
    })
  } else {
    return new Response('Something went wrong', {
      headers: { 'content-type': 'text/plain' }
    })
  }
}
```

```markdown
Current player: 

<img src="https://gh-tik-tak-toe.jackcrane.workers.dev/current-player?escape-cache">

<table>
  <tr>
    <td>
      <a href="https://gh-tik-tak-toe.jackcrane.workers.dev/move/0">
        <img src="https://gh-tik-tak-toe.jackcrane.workers.dev/image/0?escape-cache">
      </a>
    </td>
    <td>
      <a href="https://gh-tik-tak-toe.jackcrane.workers.dev/move/1">
        <img src="https://gh-tik-tak-toe.jackcrane.workers.dev/image/1?escape-cache">
      </a>
    </td>
    <td>
      <a href="https://gh-tik-tak-toe.jackcrane.workers.dev/move/2">
        <img src="https://gh-tik-tak-toe.jackcrane.workers.dev/image/2?escape-cache">
      </a>
    </td>
  </tr>
  <tr>
    <td>
      <a href="https://gh-tik-tak-toe.jackcrane.workers.dev/move/3">
        <img src="https://gh-tik-tak-toe.jackcrane.workers.dev/image/3?escape-cache">
      </a>
    </td>
    <td>
      <a href="https://gh-tik-tak-toe.jackcrane.workers.dev/move/4">
        <img src="https://gh-tik-tak-toe.jackcrane.workers.dev/image/4?escape-cache">
      </a>
    </td>
    <td>
      <a href="https://gh-tik-tak-toe.jackcrane.workers.dev/move/5">
        <img src="https://gh-tik-tak-toe.jackcrane.workers.dev/image/5?escape-cache">
      </a>
    </td>
  </tr>
  <tr>
    <td>
      <a href="https://gh-tik-tak-toe.jackcrane.workers.dev/move/6">
        <img src="https://gh-tik-tak-toe.jackcrane.workers.dev/image/6?escape-cache">
      </a>
    </td>
    <td>
      <a href="https://gh-tik-tak-toe.jackcrane.workers.dev/move/7">
        <img src="https://gh-tik-tak-toe.jackcrane.workers.dev/image/7?escape-cache">
      </a>
    </td>
    <td>
      <a href="https://gh-tik-tak-toe.jackcrane.workers.dev/move/8">
        <img src="https://gh-tik-tak-toe.jackcrane.workers.dev/image/8?escape-cache">
      </a>
    </td>
  </tr>
</table>

(You will need to reload the page after you make a move, otherwise new images will not be fetched)

[Reset Game](https://gh-tik-tak-toe.jackcrane.workers.dev/reset)
```

<script data-name="BMC-Widget" data-cfasync="false" src="https://cdnjs.buymeacoffee.com/1.0.0/widget.prod.min.js" data-id="jackcrane" data-description="Support me on Buy me a coffee!" data-message="Feeling generous?" data-color="#FFDD00" data-position="Right" data-x_margin="18" data-y_margin="18"></script>