<template>
  <div class="row window-width window-height" ref="game"/>
</template>

<script setup lang="ts">
import {Application, Container, Graphics, Point, SimpleRope, Sprite, Texture} from 'pixi.js';
import {onMounted, ref} from 'vue';

interface GameProps {
  height: number;
}

const props = defineProps<GameProps>();

// The application will create a renderer using WebGL, if possible,
// with a fallback to a canvas render. It will also setup the ticker
// and the root stage PIXI.Container
const game = ref();
// The application will create a canvas element for you that you
// can then insert into the DOM

function getRandom(min: number, max: number) {
  return Math.random() * (max - min) + min;
}

function addObstacles(app: any) {
  const texture = Texture.from('https://sun9-1.userapi.com/s/v1/if1/KeAwylEbnnol-MTy_OMgwJGbdCqjJ8VOjnYXKBSp6QtB2GgP04paP03q-vq1noMYpzHcWia6.jpg?quality=96&crop=0,39,1616,1616&as=50x50,100x100,200x200,400x400&ava=1&u=_2ImjxZGdwGYFR8mUAW95ZPg6aE-zwV9vib8XulIPWw&cs=200x200');
  const obstacle = new Sprite(texture);
  obstacle.position.set(app.screen.width, app.screen.height / 2);

// Add the obstacle to the stage
  app.stage.addChild(obstacle);

// Animation loop
  app.ticker.add((delta: any) => {
    // Move the obstacle from right to left
    obstacle.x -= 5 * delta;

    // Check if the obstacle is out of the screen on the left
    if (obstacle.x + obstacle.width < 0) {
      // Reset the obstacle to the right side
      obstacle.x = app.screen.width;
      obstacle.y = getRandom(100, app.screen.height - 100);
    }
  });
}

// Function to create the snake
function createSnake(app: any) {
  let count = 0;

// build a rope!
  const ropeLength = 918 / 20;

  const points: any = [];

  for (let i = 0; i < 20; i++)
  {
    points.push(new Point(i * ropeLength, 0));
  }

  const strip = new SimpleRope(Texture.from('https://pixijs.com/assets/snake.png'), points);

  strip.x = -459;

  const snakeContainer: any = new Container();

  snakeContainer.x = 400;
  snakeContainer.y = 300;

  snakeContainer.scale.set(800 / 1100);
  app.stage.addChild(snakeContainer);

  snakeContainer.addChild(strip);

  app.ticker.add(() =>
  {
    count += 0.1;

    // make the snake
    for (let i = 0; i < points.length; i++)
    {
      points[i].y = Math.sin((i * 0.5) + count) * 30;
      points[i].x = i * ropeLength + Math.cos((i * 0.3) + count) * 20;
    }
  });
  return { snakeContainer, strip };
}

// Function to check if the snake touches an obstacle
function checkCollision(snake: any, obstacle: any) {
  const snakeBounds = snake.getBounds();
  const obstacleBounds = obstacle.getBounds();

  return (
    snakeBounds.x < obstacleBounds.x + obstacleBounds.width &&
    snakeBounds.x + snakeBounds.width > obstacleBounds.x &&
    snakeBounds.y < obstacleBounds.y + obstacleBounds.height &&
    snakeBounds.y + snakeBounds.height > obstacleBounds.y
  );
}

// Function to end the game
function endGame() {
  // Your logic to end the game goes here
  console.log('Game Over!');
}

onMounted(() => {
  // game.value.style.height = props.height + 'px';
  const app = new Application({
    resizeTo: game.value,
  });
  game.value.appendChild(app.view as any);

  addObstacles(app);

  // Create the snake
  const { snakeContainer, strip } = createSnake(app);

  // Main game loop
  app.ticker.add(() => {
    // Check for collision
    // if (checkCollision(strip, obstacle)) {
    //   endGame();
    // }
  });
});


</script>
