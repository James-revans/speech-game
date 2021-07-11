<script>
import { Animate } from 'svelte-swipeable';
  let direction = 'any';
  let willReturn = false;
  let stiffness = 0.3;
  let damping = 0.8;
  let momentum = 0.2;

  let grabbers = [1, 2, 3, 4, 5];

  let activeIndex = null;

  let grabImgA = "https://i.pinimg.com/originals/eb/1e/bc/eb1ebcf28c7b93762b33b0d06808245b.png";
  let grabImgB = "images/tea.png";

  let goalImgA = "https://thumbs.dreamstime.com/b/front-door-house-entrace-icon-cartoon-vector-illustration-graphic-design-front-door-house-entrace-icon-151525317.jpg";
  let goalImgB = "images/grandma.png";

  let items = [
    {
      goalImg: goalImgA,
      grabImg: grabImgA
    },
    {
      goalImg: goalImgB,
      grabImg: grabImgB
    }
  ]

</script>

<style>
  .wrapper {
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
    margin-right: 50px;
    display: flex;
    flex-wrap: wrap;
  }

  .grab-wrapper {
    display: flex;
    position: relative;
  }

  .grab-img {
    width: 100px;
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

</style>

<div class="wrapper">
  {#each items as {goalImg, grabImg}}
  <div class="grouping">
    <img class="goal-img" src={goalImg}/>
    <div class="grabbers">
      {#each grabbers as _}
      <Animate direction={direction} stiffness={stiffness} damping={damping} willReturn={willReturn} momentum={momentum}>
        <div class="grab-wrapper">
          <div class="grab-mask"></div>
          <img class="grab-img" src={grabImg}/>
        </div>
      </Animate>
    {/each}
    </div>
  </div>
  {/each}
</div>

