<template>
  <div>
    <h2>My Simple Game</h2>
    <!-- HUD: shows live data from reactive refs -->
    <p>Score: {{ score }}</p>
    <p>Position: {{ player.x }}, {{ player.y }}</p>
    <!-- Canvas: ref='canvas' lets us access it in JS -->
    <canvas ref="canvas" width="400" height="400"></canvas>
  </div>
</template>
<script setup>
import { ref, onMounted } from 'vue'

// ref() makes a value reactive — UI auto-updates when it changes
const canvas = ref(null)               // the canvas DOM element
const player = ref({ x: 100, y: 100 }) // player position
const score  = ref(0)                  // current score
let ctx                                 // drawing context (not reactive)

function draw() {
  ctx.clearRect(0, 0, 400, 400)  // wipe canvas each frame
  ctx.fillStyle = '#eee'
  ctx.fillRect(0, 0, 400, 400)   // draw background
  ctx.fillStyle = 'blue'
  ctx.fillRect(player.value.x, player.value.y, 20, 20) // player

  ctx.fillStyle = 'red'
ctx.fillRect(item.value.x, item.value.y, 10, 10)

}

function move(e) {
  const step = 10
  if (e.key === 'ArrowUp')    player.value.y -= step
  if (e.key === 'ArrowDown')  player.value.y += step
  if (e.key === 'ArrowLeft')  player.value.x -= step
  if (e.key === 'ArrowRight') player.value.x += step

  // Clamp — keeps player inside the canvas
  player.value.x = Math.max(0, Math.min(380, player.value.x))
  player.value.y = Math.max(0, Math.min(380, player.value.y))

  score.value++  // increase score on every move

  // AABB collision: check if all 4 edges overlap
if (
  player.value.x     < item.value.x + 10 &&
  player.value.x + 20 > item.value.x      &&
  player.value.y     < item.value.y + 10 &&
  player.value.y + 20 > item.value.y
) {
  score.value += 10                    // +10 bonus points!
  item.value.x = Math.random() * 380  // respawn item
  item.value.y = Math.random() * 380
}

}

function loop() {
  draw()
  requestAnimationFrame(loop) // ~60 fps game loop
}

onMounted(() => {
  // onMounted = safe to touch the DOM here
  ctx = canvas.value.getContext('2d')
  window.addEventListener('keydown', move)
  loop()
})
// Random starting position for the red collectible
const item = ref({
  x: Math.random() * 380,
  y: Math.random() * 380
})

</script>

<style>
canvas { border: 2px solid black; }
</style>
