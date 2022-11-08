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
    <h1 class="description">{{ localHours.length }} active hours</h1>
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
  // this is random color generator to make activeHours color different
  // return "#" + ("00000" + Math.floor(Math.random() * Math.pow(16, 6)).toString(16)).slice(-6)+'60';

  // use this for show conflict of activeHours
  return 'rgb(255 127 0 / 36%)';
}

function setDate() {
  //this is for fun

  const now = new Date();
  const seconds = now.getSeconds();
  const secondsDegrees = ((seconds / 60) * 360) + 90;
  secondHandTransform.value = `rotate(${secondsDegrees}deg)`;

  const mins = now.getMinutes();
  const minsDegrees = ((mins / 60) * 360) + ((seconds / 60) * 6) + 90;
  minsHandTransform.value = `rotate(${minsDegrees}deg)`;

  const hour = now.getHours();
  const hourDegrees = ((hour / 12) * 360) + ((mins / 60) * 30) + 90;
  hourHandTransform.value = `rotate(${hourDegrees}deg)`;
}

setInterval(setDate, 1000);

setDate();

</script>

<style lang="less">
@import "src/components/Clock/assets/css/main";
</style>