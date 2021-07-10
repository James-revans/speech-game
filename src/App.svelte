<script>
import { Animate } from 'svelte-swipeable';
  let direction = 'any';
  let willReturn = false;
  let stiffness = 0.3;
  let damping = 0.8;
  let momentum = 0.2;

  let arr = [1, 2, 3, 4, 5];

  let activeIndex = null;
</script>

<style>
  .wrapper {
    max-width: 900px;
    display: flex;
    justify-content: space-between;
    margin: 0 auto;
  }
  .big-img {
    width: 50%;
  }

  .item-wrapper {
    /* max-width: 900px; */
    display: flex;
    justify-content: center;
    margin: 0 auto; 
  }

  .item {
    width: 100px;
    position: relative;
    z-index: 0;
  }

  .keys {
    margin-right: 50px;
    display: flex;
  }

  .teas {
    margin-left: 50px;
    display: flex;
  }

  .grab-wrapper {
    position: relative;
  }

  .grabber {
    position: absolute;
    width: 100%;
    height: 100%;
    z-index: 1000;
  }

</style>

<div class="wrapper">
  <img class="big-img door" src="https://thumbs.dreamstime.com/b/front-door-house-entrace-icon-cartoon-vector-illustration-graphic-design-front-door-house-entrace-icon-151525317.jpg"/>
  <img class="big-img grandma" src="images/grandma.png"/>
</div>

<div class="item-wrapper">
  <div class="keys">
  {#each arr as item}
    <Animate direction={direction} stiffness={stiffness} damping={damping} willReturn={willReturn} momentum={momentum}>
      <div class="grab-wrapper">
        <div class="grabber"></div>
        <img class="item" src="https://i.pinimg.com/originals/eb/1e/bc/eb1ebcf28c7b93762b33b0d06808245b.png"/>
      </div>
    </Animate>
  {/each}
  </div>
  <div class="teas">
  {#each arr as item}
    <Animate direction={direction} stiffness={stiffness} damping={damping} willReturn={willReturn} momentum={momentum}>
      <div class="grab-wrapper">
        <div class="grabber"></div>
        <img class="item" src="images/tea.png"/>
      </div>
    </Animate>
  {/each}
  </div>
</div>
