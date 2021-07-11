<script>
import { Animate } from 'svelte-swipeable';
import swipe from './svelte-swipeable/main.js';
import ConfettiGenerator from 'confetti-js';
import confettiSettings from './confetti-config.js';

let direction = 'any';
let willReturn = false;
let stiffness = 0.3;
let damping = 0.8;
let momentum = 0.2;

let activeIndex = null;

let grabImgA = "https://i.pinimg.com/originals/eb/1e/bc/eb1ebcf28c7b93762b33b0d06808245b.png";
let grabImgB = "images/tea.png";

let goalImgA = "https://thumbs.dreamstime.com/b/front-door-house-entrace-icon-cartoon-vector-illustration-graphic-design-front-door-house-entrace-icon-151525317.jpg";
let goalImgB = "images/grandma.png";

let refGoalA;
let refGoalB;

let items = [
  {
    goalImg: goalImgA,
    grabImg: grabImgA,
    refGoal: refGoalA
  },
  {
    goalImg: goalImgB,
    grabImg: grabImgB,
    refGoal: refGoalB
  }
]

let completedItems = {}
let isDone = false;
let grabbers = [1, 2, 3, 4, 5];

function handleSwipe(event) {
}

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
  if(Object.entries(completedItems).length == (grabbers.length * 2)) {
    console.log("CONFETTI!!");
    let confetti = new ConfettiGenerator(confettiSettings);
    confetti.render();
    isDone = true;
  }
}
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Luckiest+Guy&display=swap');

  .game {    
    max-width: 1000px;
    display: flex;
    justify-content: space-between;
    align-items: baseline;
    margin: 0 auto;
  }

  .grouping {
    width: 50%;
    justify-content: center;
    margin: 0 auto; 
  }

  .goal-img {
    width: 100%;
  }

  .grabbers {
    margin: 60px 20px;
    display: flex;
    flex-wrap: no-wrap;
  }

  .grab-wrapper {
    position: relative;
    opacity: 1;
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

</style>

<div class="wrapper">
  <canvas id="my-canvas" class="confetti"></canvas>
  <h1 class="complete-message" class:show={isDone}>YOU DID IT!!</h1>
  <div class="game" class:isDone={isDone}>
    {#each items as {goalImg, grabImg, refGoal}, i}
    <div class="grouping">
      <div class="goal" bind:this={refGoal}>
        <img class="goal-img" src={goalImg}/>
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
              on:swipeMove={handleSwipe}
              on:swipeEnd={(e) => handleSwipeEnd(e, refGoal, `${i}${j}`)}
              ></div>
              <img class="grab-img" src={grabImg}/>
          </div>
        </Animate>
      {/each}
      </div>
    </div>
    {/each}
  </div>
</div>

