<script setup lang="tsx">
import { onMounted, onUnmounted } from 'vue';
import Clock from './components/clock';

let times = $ref<number[]>([]);
const time = $computed(() => times.map(t => (t < 10 ? `0${t}` : t)).join(':'));
let count = $ref(0);

let timer: number;

const setTime = () => {
  const date = new Date();
  times = [date.getHours(), date.getMinutes(), date.getSeconds()];
};

const handleCountChange = (outerCount: number) => {
  count = outerCount;
};

onMounted(() => {
  setTime();
  timer = setInterval(setTime, 1000);
});

onUnmounted(() => {
  clearInterval(timer);
});

const JSX = () => (
  <div>
    <Clock
      time={time}
      outerCount={count}
      footer={<span>{time}</span>}
      renderTime={t => <span>{t}</span>}
      onOuterCountChange={handleCountChange}
    >
      CLOCK
    </Clock>
  </div>
);
</script>

<template>
  <JSX />
</template>
