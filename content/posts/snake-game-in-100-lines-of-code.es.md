---
title: "Programé un Snake con solo 100 líneas de código"
slug: "juego-snake-100-lineas-de-codigo"
description: "Esta es mi implementación del famoso juego Snake. He tratado de hacerla lo más sencilla posible, y por eso con una cantidad tan pequeña de código."
summary: "Esta es mi implementación del famoso juego Snake. He tratado de hacerla lo más sencilla posible, y por eso con una cantidad tan pequeña de código."
date: 2022-11-19T19:22:38+01:00
draft: false
---

En este post explicaré cómo programé uno de los juegos más emblemáticos de la historia, en solo 100 líneas de código. Pero antes, un poco de contexto:

Para crear un juego, habitualmente utilizaremos un motor gráfico, por ejemplo Unity o Unreal Engine, entre otros. Dentro de un juego tendremos **entidades** y **sistemas**. Pero, ¿Qué son las entidades y los sistemas?

Si has realizado alguna vez POO (Programación Orientada a Objetos) te será más fácil de entender, pero aún así, comencemos con las entidades.

**Una entidad** podría ser la serpiente, en el caso del juego que hablamos, pero otra entidad podría ser una manzana. Tanto la manzana como la serpiente tienen una posición X e Y, que definen en qué parte del mapa están.

Y **un sistema** sería el que se encarga de actualizar los datos del juego. Por ejemplo, existe el sistema de físicas, que cambia la posición X e Y de la serpiente, según si se tiene que mover hacia arriba, abajo, izquierda, o derecha.

## Entonces, ¿cómo has programado el juego?

Vamos a dejar de hacer un lío, tanto sistema, entidad, etc. ¡Y vamos a ver código! Las primeras líneas simplemente son constantes (configuraciones del juego):

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

Después creamos tres objetos que son las "cosas" que veremos en la pantalla (**entidades**).

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

¡Y ahora vamos con el primer **sistema**! Se trata del **sistema de renderizado**, básicamente, dibujar las “cosas“ (**entidades**) en la pantalla:

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

Y, por supuesto, tenemos una función principal que llama a todas ellas. Lo primero que hace es dibujar toda la pantalla de blanco, y después el resto.

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

¡Ahora tenemos que mover a la serpiente! Tenemos que modificar las posiciones X e Y de la serpiente. Esto se hace con la siguiente función (lo que llamaríamos **sistema de físicas**):

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

Y lo siguiente que haremos será comprobar si la serpiente ha muerto, o si se ha comido la manzana (sí, ya sabes por dónde vamos, esto se llama **sistema de colisiones**).

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

Por último, para que el jugador pueda moverse por el mapa, necesitamos que el juego interactue con un controlador, en este caso el teclado. Esto se llama **sistema de input**.

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

Ahora ya tenemos los cuatro sistemas que contienen el 99% de los videojuegos, después puedes añadirle los que quieras, según cuál sea la intención del juego. En este caso necesitamos el **sistema de spawn**, para hacer aparecer a la manzana cada vez que la serpiente se coma una.

```js
function generateFood() {
    food.x = Math.floor(Math.random() * (GAME_WIDTH / SQUARE_SIZE)) * SQUARE_SIZE;
    food.y = Math.floor(Math.random() * (GAME_HEIGHT / SQUARE_SIZE)) * SQUARE_SIZE;
}
```

Lo único que nos falta para que esto sea un juego convencional es un main loop, y un estado inicial. Esto sirve para saber cómo empieza el juego, y ejecutar todos los sistemas en orden. Además he añadido una función auxiliar (isGameOver) para saber si la serpiente está muerta. Por lo general haríamos un **sistema de vida** para controlar esto, pero este juego es tan simple que no lo necesita.

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

En este caso, el main loop no es un bucle for, o while, sino que aprovechamos que JavaScript está diseñado para ser programado mediante eventos y le decimos al final del main loop que vuelva a llamarse a sí mismo dentro de un tiempo (en este caso 100ms).

Por último, cabe comentar que comúnmente se ejecutan todos los sistemas en el main loop, en este caso no se está ejecutando el **sistema de input** porque JavaScript permite poner un listener en paralelo. Esto lo hacemos con el siguiente listener:

```js
document.addEventListener('keydown', handleKeyDown);
document.getElementById('start').addEventListener('click', startGame);
```

## Ahora quiero aprender más, ¿cómo creo otro juego?

Ahora que has entendido el concepto de entidad y sistema, ya puedes empezar a programar tu propia idea de videojuego. Ya sea sin motor gráfico (como lo acabamos de ver ahora), o utilizando un motor profesional, ya sea Unity, Game Maker, Unreal Engine, o cual sea.

Pero antes, si quieres ver más en profundidad te aportaré dos recursos:

1. [Mi TFG](http://rua.ua.es/dspace/handle/10045/115983) (también [en GitHub](https://github.com/arturo-source/tfg-motor-juego-y-ia)), el cual está dividido en tres partes, en la primera se hace un motor de videojuegos, en la segunda se crea un juego usando este motor, y en la tercera se añade inteligencia artificial a este juego creado.
2. [El curso de mi tutor del TFG](https://www.youtube.com/watch?v=7YBzHJJYpZo&list=PLmxqg54iaXrhTqZxylLPo0nov0OoyJqiS). Con el cual aprendí todo lo que sé de videojuegos, y si te interesa aprender a hacer tu propio motor de videojuegos desde cero, es el mejor recurso que vas a tener.

## Prueba el juego aquí mismo

<button id="start" style="border: 1px solid black; padding: 10px; border-radius: 10px;">Start Playing</button>
<div>
    <canvas id="game" style="border: 1px solid black;"></canvas>
</div>
<script>const SQUARE_SIZE=20;const GAME_WIDTH=800;const GAME_HEIGHT=800;const canvas=document.getElementById('game');const ctx=canvas.getContext('2d');canvas.width=GAME_WIDTH;canvas.height=GAME_HEIGHT;var snake={body:[{x:400,y:400},],nextMove:'right',};var food={x:0,y:0,};var game={score:0,speed:100,isOver:false,};function drawSnake(){ctx.fillStyle='#3a5a40';ctx.fillRect(snake.body[0].x,snake.body[0].y,SQUARE_SIZE,SQUARE_SIZE);ctx.fillStyle='#a3b18a';snake.body.slice(1).forEach(function(part){ctx.fillRect(part.x,part.y,SQUARE_SIZE,SQUARE_SIZE)})}function drawFood(){ctx.fillStyle='red';ctx.fillRect(food.x,food.y,SQUARE_SIZE,SQUARE_SIZE)}function drawScore(){ctx.fillStyle='black';ctx.font='20px Arial';ctx.fillText('Score: '+game.score,10,30)}function drawGameOver(){ctx.fillStyle='black';ctx.font='50px Arial';ctx.fillText('Game Over',200,400)}function drawGame(){ctx.clearRect(0,0,GAME_WIDTH,GAME_HEIGHT);drawSnake();drawFood();drawScore();if(game.isOver){drawGameOver()}}function moveSnake(){var head=snake.body[0];var newHead={x:head.x,y:head.y,};switch(snake.nextMove){case'right':newHead.x+=SQUARE_SIZE;break;case'left':newHead.x-=SQUARE_SIZE;break;case'up':newHead.y-=SQUARE_SIZE;break;case'down':newHead.y+=SQUARE_SIZE;break}snake.body.unshift(newHead);snake.body.pop()}function isSnakeOutOfGame(){var head=snake.body[0];return head.x<0||head.x>=GAME_WIDTH||head.y<0||head.y>=GAME_HEIGHT}function isSnakeEatingFood(){var head=snake.body[0];return head.x===food.x&&head.y===food.y}function isSnakeEatingItself(){var head=snake.body[0];return snake.body.slice(1).some(function(part){return part.x===head.x&&part.y===head.y})}function isGameOver(){return isSnakeOutOfGame()||isSnakeEatingItself()}function generateFood(){food.x=Math.floor(Math.random()*(GAME_WIDTH/SQUARE_SIZE))*SQUARE_SIZE;food.y=Math.floor(Math.random()*(GAME_HEIGHT/SQUARE_SIZE))*SQUARE_SIZE}function handleKeyDown(e){e.preventDefault();switch(e.key){case'ArrowLeft':if(snake.nextMove!=='right'){snake.nextMove='left'}break;case'ArrowUp':if(snake.nextMove!=='down'){snake.nextMove='up'}break;case'ArrowRight':if(snake.nextMove!=='left'){snake.nextMove='right'}break;case'ArrowDown':if(snake.nextMove!=='up'){snake.nextMove='down'}break}}function main(){if(isGameOver()){game.isOver=true;drawGameOver();return}if(isSnakeEatingFood()){snake.body.push({});game.score+=1;game.speed-=1;generateFood()}moveSnake();drawGame();setTimeout(main,game.speed)}function startGame(){snake.body=[{x:GAME_WIDTH/2,y:GAME_HEIGHT/2},];game={score:0,speed:100,};generateFood();main()}document.addEventListener('keydown',handleKeyDown);document.getElementById('start').addEventListener('click',startGame);</script>
