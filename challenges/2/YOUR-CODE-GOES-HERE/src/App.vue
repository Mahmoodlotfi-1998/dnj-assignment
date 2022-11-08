<template>
  <Clock :activeHours="activeHours"/>
</template>


<script setup lang="ts">
import Clock from "./components/Clock.vue";
import {ref} from "vue";

const activeHours =ref([{start: 5, end: 13}, {start: 12, end: 24}])

</script>
