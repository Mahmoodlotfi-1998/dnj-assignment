<template>
  <div class="container">
    <div class="clock-component">
      <div class="clock">
        <div class="outer-clock-face">
          <div class="marking marking-one"></div>
          <div class="marking marking-two"></div>
          <div class="marking marking-three"></div>
          <div class="marking marking-four"></div>
          <div class="marking marking-five"></div>
          <div class="marking marking-six"></div>
          <div class="marking marking-seven"></div>
          <div class="marking marking-eight"></div>
          <div class="marking marking-nine"></div>
          <div class="marking marking-ten"></div>
          <div class="marking marking-eleven"></div>
          <div class="inner-clock-face">
            <div class="hand hour-hand" :style="{transform:hourHandTransform}"></div>
            <div class="hand min-hand" :style="{transform:minsHandTransform}"></div>
            <div class="hand second-hand" :style="{transform:secondHandTransform}"></div>
          </div>
        </div>
      </div>
      <svg viewBox="0 0 100 100" width="300">
        <g transform="rotate(270 50 50)" fill="none" stroke-width="5">
          <template v-for="item in localHours">

            <circle cx="50" cy="50" r="45" :stroke="item.color"
                    :stroke-dashoffset="(item.start)*-1" :stroke-dasharray="`${item.end-item.start} 24`"
                    pathLength="24"/>
            <!--            <circle v-if="item.start+(item.end-item.start)>12 && item.start<12 && item.end" cx="50" cy="50" r="45"-->
            <!--                    :stroke="item.color"-->
            <!--                    :stroke-dashoffset="0" :stroke-dasharray="`${(item.start+(item.end-item.start))-12} 12`"-->
            <!--                    pathLength="12"/>-->
          </template>
        </g>
      </svg>
    </div>
    <h1 class="description">{{localHours.length}} active hours</h1>
  </div>
</template>

<script setup lang="ts">

import {computed, ref} from "vue";

interface ITimeRange {
  start: number; // 0 - 24
  end: number; // 0 - 24
}

interface IProps {
  activeHours: ITimeRange[];
}

const props = withDefaults(defineProps<IProps>(), {
  activeHours: []
})

const localHours = computed(() => {
  return props.activeHours.map((item) => {
    return {
      ...item,
      color: getRandomColor()
    }
  })
})

const secondHandTransform = ref<string>('')
const minsHandTransform = ref<string>('')
const hourHandTransform = ref<string>('')


function getRandomColor() {
  // return "#" + ("00000" + Math.floor(Math.random() * Math.pow(16, 6)).toString(16)).slice(-6)+'60';
  return 'rgb(255 127 0 / 36%)';
}

function setDate() {
  //this is for fun

  const now = new Date();
  const seconds = now.getSeconds();
  const secondsDegrees = ((seconds / 60) * 360) + 90;
  secondHandTransform.value = `rotate(${secondsDegrees}deg)`;

  const mins = now.getMinutes();
  const minsDegrees = ((mins / 60) * 360) + ((seconds/60)*6) + 90;
  minsHandTransform.value = `rotate(${minsDegrees}deg)`;

  const hour = now.getHours();
  const hourDegrees = ((hour / 12) * 360) + ((mins/60)*30) + 90;
  hourHandTransform.value = `rotate(${hourDegrees}deg)`;
}

setInterval(setDate, 1000);

setDate();

</script>

<style lang="less">

//use clock style from https://dev.to/code_mystery/simple-analog-clock-using-html-css-javascript-2c6a

.description{
  color: #282828;
}
body{
  margin: 0;
}
.container{
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  height: 100vh;
  width: 100vw;
  gap: 10px;
}

.clock-component {
  position: relative;
  svg {
    position: absolute;
    top: -25px;
    left: -25px;
  }
  .clock {
    width: 250px;
    height: 250px;
    border-radius: 50%;
    position: relative;
  }

}

.outer-clock-face {
  position: relative;
  width: 100%;
  height: 100%;
  border-radius: 50%;
  background: #282828;

  &:after{
    -webkit-transform: rotate(90deg);
    -moz-transform: rotate(90deg);
    transform: rotate(90deg)
  }
}

.outer-clock-face{
  &:before,&:after,.marking{
    content: '';
    position: absolute;
    width: 5px;
    height: 100%;
    background: #1df52f;
    z-index: 0;
    left: 49%;
  }
  .marking {
    background: #bdbdcb;
    width: 3px;
  }
  .marking.marking-one {
    -webkit-transform: rotate(15deg);
    -moz-transform: rotate(15deg);
    transform: rotate(15deg)
  }

  .marking.marking-two {
    -webkit-transform: rotate(30deg);
    -moz-transform: rotate(30deg);
    transform: rotate(30deg)
  }

  .marking.marking-three {
    -webkit-transform: rotate(45deg);
    -moz-transform: rotate(45deg);
    transform: rotate(45deg)
  }

  .marking.marking-four {
    -webkit-transform: rotate(60deg);
    -moz-transform: rotate(60deg);
    transform: rotate(60deg)
  }
  .marking.marking-five {
    -webkit-transform: rotate(75deg);
    -moz-transform: rotate(75deg);
    transform: rotate(75deg)
  }

  .marking.marking-six {
    -webkit-transform: rotate(90deg);
    -moz-transform: rotate(90deg);
    transform: rotate(90deg)
  }

  .marking.marking-seven {
    -webkit-transform: rotate(105deg);
    -moz-transform: rotate(105deg);
    transform: rotate(105deg)
  }

  .marking.marking-eight {
    -webkit-transform: rotate(120deg);
    -moz-transform: rotate(120deg);
    transform: rotate(120deg)
  }
  .marking.marking-nine {
    -webkit-transform: rotate(135deg);
    -moz-transform: rotate(135deg);
    transform: rotate(135deg)
  }

  .marking.marking-ten {
    -webkit-transform: rotate(150deg);
    -moz-transform: rotate(150deg);
    transform: rotate(150deg)
  }
  .marking.marking-eleven {
    -webkit-transform: rotate(165deg);
    -moz-transform: rotate(165deg);
    transform: rotate(165deg)
  }
}

.inner-clock-face {
  position: absolute;
  top: 10%;
  left: 10%;
  width: 80%;
  height: 80%;
  background: #282828;
  -webkit-border-radius: 100%;
  -moz-border-radius: 100%;
  border-radius: 100%;
  z-index: 1;

  &:before {
    content: '';
    position: absolute;
    top: 50%;
    left: 50%;
    width: 16px;
    height: 16px;
    border-radius: 18px;
    margin-left: -9px;
    margin-top: -6px;
    background: #4d4b63;
    z-index: 11;
  }

}

.hand {
  width: 50%;
  right: 50%;
  height: 6px;
  background: #61afff;
  position: absolute;
  top: 50%;
  border-radius: 6px;
  transform-origin: 100%;
  transform: rotate(90deg);
  transition-timing-function: cubic-bezier(0.1, 2.7, 0.58, 1);

}

.hour-hand {
  width: 30%;
  z-index: 3;
}

.min-hand {
  height: 3px;
  z-index: 10;
  width: 40%;
}

.second-hand {
  background: #ee791a;
  width: 45%;
  height: 2px;
}
</style>