<script>
import { Animate } from 'svelte-swipeable';
import swipe from './svelte-swipeable/main.js';
import ConfettiGenerator from 'confetti-js';
import confettiSettings from './confetti-config.js';
import data from './minimal-pairs.js';
console.log(data);

let direction = 'any';
let willReturn = false;
let stiffness = 0.3;
let damping = 0.8;
let momentum = 0.2;

let activePair = 0;
let showOptions = false;

let refGoal0;
let refGoal1;
let refs = {
  refGoal0: null,
  refGoal1: null
}

let completedItems = {}
let isDone = false;
let grabbers = [1, 2, 3, 4, 5];

function handleSwipeEnd(event, refGoal, item) {
  let { top, bottom, left, right } = refGoal.getBoundingClientRect()
  let { x, y } = event.detail;
  let threshX = x > left && x < right;
  let threshY = y > top && y < bottom;
  if (threshX && threshY) {
    completedItems[item] = true;
    checkIfDone();
  }
}

function checkIfDone() {
  if (Object.entries(completedItems).length == (grabbers.length * 2)) {
    console.log("CONFETTI!!");
    let confetti = new ConfettiGenerator(confettiSettings);
    confetti.render();
    isDone = true;
  }
}
</script>

<style type="text/scss">
@import url('https://fonts.googleapis.com/css2?family=Luckiest+Guy&display=swap');

.game {
  display: flex;
  justify-content: space-between;
  align-items: baseline;
  margin: 0 auto;
  .grouping {
    display: flex;
    flex-direction: column;
    width: 50%;
    margin: 0 auto; 
  }
}

.align-0 {
  align-items: flex-end;
}

.align-1 {
  align-items: flex-start;
}

.goal-img {
  max-width: 500px;
}

.grabbers {
  margin: 60px 20px;
  display: flex;
  flex-wrap: wrap;
}

.grab-wrapper {
  position: relative;
  opacity: 1;
  width: 150px;
  &:hover {
    cursor: grab;
  }
  &:active {
    cursor: grabbing;
  }
}

.grab-img {
  width: 100%;
  position: relative;
  z-index: 0;
}

.grab-mask {
  position: absolute;
  width: 100%;
  height: 100%;
  z-index: 1000;
}

.grab-mask, .grab-img, .goal-img {
  user-select: none;
}

.hide {
  opacity: 0;
  transition: 1s;
}

.show {
  opacity: 1 !important;
  transition: 1s;
  z-index: 1000;
}

.isDone {
  opacity: 0.3;
  transition: 1s;
}

.confetti {
  position: absolute;
  width: 100vw;
  height: 100vh;
}

.complete-message {
  color: orange;
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -120%);
  font-size: 100px;
  font-family: 'Luckiest Guy', cursive;
  opacity: 0;
  text-shadow: 2px 2px 8px black;
}

.options{
  position: fixed;
  z-index: 1000;
  top: 10px;
  left: 10px;
  .options-toggle {
    margin-right: 20px;
    padding: 10px 20px;
    background-color: rgb(255, 254, 169);
    border: solid thin gray;
    font-size: 15px;
    &:hover {
      cursor: pointer;
    }
  }
  .option-item {
    margin-right: 20px;
    padding: 10px 20px;
    background-color: rgb(169, 238, 255);
    border: solid thin gray;
    font-size: 15px;
    &:hover {
      cursor: pointer;
    }
  }
}

</style>

<canvas id="my-canvas" class="confetti"></canvas>
<div class="wrapper">
  <h1 class="complete-message" class:show={isDone}>YOU DID IT!!</h1>
  <div class="game" class:isDone={isDone}>
    {#each data[activePair] as {goal, grab}, i}
    <div class="grouping align-{i}">
      <div class="goal" bind:this={refs[`refGoal${i}`]}>
        <img class="goal-img" src={goal}/>
      </div>
      <div class="grabbers">
        {#each grabbers as grabber, j}
        <Animate
          direction={direction} 
          stiffness={stiffness} 
          damping={damping} 
          willReturn={willReturn}
          momentum={momentum}>
          <div class="grab-wrapper" class:hide={completedItems[`${i}${j}`]}>
              <div
              class="grab-mask"
              use:swipe
              on:swipeEnd={(e) => handleSwipeEnd(e, refs[`refGoal${i}`], `${i}${j}`)}
              ></div>
              <img class="grab-img" src={grab}/>
          </div>
        </Animate>
      {/each}
      </div>
    </div>
    {/each}
  </div>
</div>

<div class="options">
<button class="options-toggle" on:click="{() => {showOptions = !showOptions}}">Options</button>

{#if showOptions}

{#each data as minimalPair, i}
  <button class="option-item" on:click="{() => {activePair = i}}">{minimalPair[0].name + '/' + minimalPair[1].name}</button>
{/each}
{/if}

</div>
