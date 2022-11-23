---
title: "I created Snake Game in 100 Lines of Code"
description: "Let's see how to code one of the most known games in the world in only 100 lines of code."
summary: "Let's see how to code one of the most known games in the world in only 100 lines of code."
date: 2022-11-19T19:22:44+01:00
draft: false
---

I will explain how I programmed one of the most emblematic games in history in this post, in just 100 lines of code. But first, a bit of context:

To create a game, we will usually use a game engine, for example Unity or Unreal Engine, among others. Within a game we will have **entities** and **systems**. But what are entities and systems?

If you have ever done OOP (Object Oriented Programming) it will be easier for you to understand, but still, let's start with entities.

**An entity** could be the snake, in the case of the game we are talking about, but another entity could be an apple. Both the apple and the snake have an X and Y position, which define where they are on the map.

And **a system** would be the one that is in charge of updating the game data. For example, there is the physics system, which changes the X and Y position of the snake, depending on whether it has to move up, down, left, or right.

## Step by step, how to create the game

Let's stop making a mess, both system, entity, etc. And let's see code! The first few lines are simply constants (game settings):

```js
const SQUARE_SIZE = 20;
const GAME_WIDTH = 800;
const GAME_HEIGHT = 800;

const canvas = document.getElementById('game');
const ctx = canvas.getContext('2d');

// set width and height full screen
canvas.width = GAME_WIDTH;
canvas.height = GAME_HEIGHT;
```

Then we create three objects that are the "things" that we will see on the screen (**entities**).

```js
var snake = {
    body: [
        { x: 400, y: 400 },
    ],
    nextMove: 'right',
};

var food = {
    x: 0,
    y: 0,
};

var game = {
    score: 0,
    speed: 100,
    isOver: false,
};
```

And now we come to the first **system**! This is the **rendering system**, basically drawing the “things” (**entities**) on the screen:

```js
function drawSnake() {
    // Draw head
    ctx.fillStyle = '#3a5a40';
    ctx.fillRect(snake.body[0].x, snake.body[0].y, SQUARE_SIZE, SQUARE_SIZE);

    // Draw body
    ctx.fillStyle = '#a3b18a';
    snake.body.slice(1).forEach(function (part) {
        ctx.fillRect(part.x, part.y, SQUARE_SIZE, SQUARE_SIZE);
    });
}

function drawFood() {
    ctx.fillStyle = 'red';
    ctx.fillRect(food.x, food.y, SQUARE_SIZE, SQUARE_SIZE);
}

function drawScore() {
    ctx.fillStyle = 'black';
    ctx.font = '20px Arial';
    ctx.fillText('Score: ' + game.score, 10, 30);
}

function drawGameOver() {
    ctx.fillStyle = 'black';
    ctx.font = '50px Arial';
    ctx.fillText('Game Over', 200, 400);
}
```

And of course we have a main function that calls all of them. The first thing it does is draw the entire screen white, and then the rest.

```js
function drawGame() {
    ctx.clearRect(0, 0, GAME_WIDTH, GAME_HEIGHT);
    drawSnake();
    drawFood();
    drawScore();
    if (game.isOver) {
        drawGameOver();
    }
}
```

Now we have to move the snake! We have to modify the X and Y positions of the snake. This is done with the following function (what we would call **physics system**):

```js
function moveSnake() {
    var head = snake.body[0];
    var newHead = {
        x: head.x,
        y: head.y,
    };
    switch (snake.nextMove) {
        case 'right':
            newHead.x += SQUARE_SIZE;
            break;
        case 'left':
            newHead.x -= SQUARE_SIZE;
            break;
        case 'up':
            newHead.y -= SQUARE_SIZE;
            break;
        case 'down':
            newHead.y += SQUARE_SIZE;
            break;
    }
    snake.body.unshift(newHead);
    snake.body.pop();
}
```

And the next thing we will do is check if the snake has died, or if it has eaten the apple (yes, you know where we are going, this is called **collision system**).

```js
// Check if snake is out of game
function isSnakeOutOfGame() {
    var head = snake.body[0];
    return head.x < 0 || head.x >= GAME_WIDTH || head.y < 0 || head.y >= GAME_HEIGHT;
}

// Check if snake is eating food
function isSnakeEatingFood() {
    var head = snake.body[0];
    return head.x === food.x && head.y === food.y;
}

// Check if snake is eating itself
function isSnakeEatingItself() {
    var head = snake.body[0];
    return snake.body.slice(1).some(function (part) {
        return part.x === head.x && part.y === head.y;
    });
}
```

Lastly, in order for the player to be able to move around the map, we need the game to interact with a controller, in this case the keyboard. This is called **input system**.

```js
function handleKeyDown(e) {
    switch (e.key) {
        case 'ArrowLeft':
            if (snake.nextMove !== 'right') {
                snake.nextMove = 'left';
            }
            break;
        case 'ArrowUp':
            if (snake.nextMove !== 'down') {
                snake.nextMove = 'up';
            }
            break;
        case 'ArrowRight':
            if (snake.nextMove !== 'left') {
                snake.nextMove = 'right';
            }
            break;
        case 'ArrowDown':
            if (snake.nextMove !== 'up') {
                snake.nextMove = 'down';
            }
            break;
    }
}
```

Now we have the four systems that contain 99% of the video games, later you can add the ones you want, depending on the intention of the game. In this case we need the **spawn system**, to make the apple appear every time the snake eats one.

```js
function generateFood() {
    food.x = Math.floor(Math.random() * (GAME_WIDTH / SQUARE_SIZE)) * SQUARE_SIZE;
    food.y = Math.floor(Math.random() * (GAME_HEIGHT / SQUARE_SIZE)) * SQUARE_SIZE;
}
```

The only thing missing for this to be a conventional game is a main loop, and an initial state. This is to know how the game starts, and run all systems in order. I have also added a helper function (isGameOver) to know if the snake is dead. Usually we would make a **life system** to control this, but this game is so simple it doesn't need it.

```js
function isGameOver() {
    return isSnakeOutOfGame() || isSnakeEatingItself();
}

// Main game loop
function main() {
    if (isGameOver()) {
        game.isOver = true;
        drawGameOver();
        return;
    }
    if (isSnakeEatingFood()) {
        snake.body.push({});
        game.score += 1;
        game.speed -= 1;
        generateFood();
    }
    moveSnake();
    drawGame();
    setTimeout(main, game.speed);
}

// Start game
function startGame() {
    snake.body = [
        { x: GAME_WIDTH / 2, y: GAME_HEIGHT / 2 },
    ];
    game = {
        score: 0,
        speed: 100,
    };

    generateFood();
    main();
}
```

In this case, the main loop is not a for loop, or while loop, but instead we take advantage of the fact that JavaScript is designed to be scripted via events and tell it at the end of the main loop to call itself again in some time (in this case, case 100ms).

Finally, it is worth mentioning that commonly all the systems are executed in the main loop, in this case the **input system** is not being executed because JavaScript allows to put a listener in parallel. We do this with the following listener:

```js
document.addEventListener('keydown', handleKeyDown);
document.getElementById('start').addEventListener('click', startGame);
```

## Some contributions to start learning

All the contributions are in Spanish, because I developed my final project in Spanish, but I think they are very useful for anyone who wants to start learning.

Now that you have understood the concept of entity and system, you can start programming your own video game idea. Either without a graphics engine (as we have just seen now), or using a professional engine, be it Unity, Game Maker, Unreal Engine, or whatever.

Both are in spanish, but you can see the code (in english) in my GitHub:

1. [My final degree project](http://rua.ua.es/dspace/handle/10045/115983) ([GitHub too](https://github.com/arturo-source/tfg-motor-juego-y-ia)), which is divided into three parts, in the first a video game engine is made, in the second a game is created using this engine, and in the third artificial intelligence is added to this created game.
2. [The course from the person who guided me](https://www.youtube.com/watch?v=7YBzHJJYpZo&list=PLmxqg54iaXrhTqZxylLPo0nov0OoyJqiS) (in Spanish). With which I learned everything I know about video games, and if you are interested in learning how to make your own video game engine from scratch, it is the best resource you will have.

## Try it out right now

<button id="start" style="border: 1px solid black; padding: 10px; border-radius: 10px;">Start Playing</button>
<div>
    <canvas id="game" style="border: 1px solid black;"></canvas>
</div>
<script>const SQUARE_SIZE=20;const GAME_WIDTH=800;const GAME_HEIGHT=800;const canvas=document.getElementById('game');const ctx=canvas.getContext('2d');canvas.width=GAME_WIDTH;canvas.height=GAME_HEIGHT;var snake={body:[{x:400,y:400},],nextMove:'right',};var food={x:0,y:0,};var game={score:0,speed:100,isOver:false,};function drawSnake(){ctx.fillStyle='#3a5a40';ctx.fillRect(snake.body[0].x,snake.body[0].y,SQUARE_SIZE,SQUARE_SIZE);ctx.fillStyle='#a3b18a';snake.body.slice(1).forEach(function(part){ctx.fillRect(part.x,part.y,SQUARE_SIZE,SQUARE_SIZE)})}function drawFood(){ctx.fillStyle='red';ctx.fillRect(food.x,food.y,SQUARE_SIZE,SQUARE_SIZE)}function drawScore(){ctx.fillStyle='black';ctx.font='20px Arial';ctx.fillText('Score: '+game.score,10,30)}function drawGameOver(){ctx.fillStyle='black';ctx.font='50px Arial';ctx.fillText('Game Over',200,400)}function drawGame(){ctx.clearRect(0,0,GAME_WIDTH,GAME_HEIGHT);drawSnake();drawFood();drawScore();if(game.isOver){drawGameOver()}}function moveSnake(){var head=snake.body[0];var newHead={x:head.x,y:head.y,};switch(snake.nextMove){case'right':newHead.x+=SQUARE_SIZE;break;case'left':newHead.x-=SQUARE_SIZE;break;case'up':newHead.y-=SQUARE_SIZE;break;case'down':newHead.y+=SQUARE_SIZE;break}snake.body.unshift(newHead);snake.body.pop()}function isSnakeOutOfGame(){var head=snake.body[0];return head.x<0||head.x>=GAME_WIDTH||head.y<0||head.y>=GAME_HEIGHT}function isSnakeEatingFood(){var head=snake.body[0];return head.x===food.x&&head.y===food.y}function isSnakeEatingItself(){var head=snake.body[0];return snake.body.slice(1).some(function(part){return part.x===head.x&&part.y===head.y})}function isGameOver(){return isSnakeOutOfGame()||isSnakeEatingItself()}function generateFood(){food.x=Math.floor(Math.random()*(GAME_WIDTH/SQUARE_SIZE))*SQUARE_SIZE;food.y=Math.floor(Math.random()*(GAME_HEIGHT/SQUARE_SIZE))*SQUARE_SIZE}function handleKeyDown(e){e.preventDefault();switch(e.key){case'ArrowLeft':if(snake.nextMove!=='right'){snake.nextMove='left'}break;case'ArrowUp':if(snake.nextMove!=='down'){snake.nextMove='up'}break;case'ArrowRight':if(snake.nextMove!=='left'){snake.nextMove='right'}break;case'ArrowDown':if(snake.nextMove!=='up'){snake.nextMove='down'}break}}function main(){if(isGameOver()){game.isOver=true;drawGameOver();return}if(isSnakeEatingFood()){snake.body.push({});game.score+=1;game.speed-=1;generateFood()}moveSnake();drawGame();setTimeout(main,game.speed)}function startGame(){snake.body=[{x:GAME_WIDTH/2,y:GAME_HEIGHT/2},];game={score:0,speed:100,};generateFood();main()}document.addEventListener('keydown',handleKeyDown);document.getElementById('start').addEventListener('click',startGame);</script>
