<script setup lang="tsx">
import { useSlots, watch } from 'vue';
import styles from './index.module.scss';

interface ClockProps {
  time: string;
  outerCount: number;
  footer: VueNode;
  renderTime(msg: string): VueNode;
  onOuterCountChange(count: number): void;
}

const { time, outerCount, footer, renderTime, onOuterCountChange } = defineProps<ClockProps>();
const { default: children } = useSlots();

let count = $ref(0);

watch($$(time), () => {
  count++;
  onOuterCountChange(outerCount + 1);
});

const JSX = () => (
  <div class={styles['clock']}>
    <div class={styles['children']}>{children?.()}</div>

    <div class={styles['main-clock']}>{time}</div>

    <div class={styles['props-boxes']}>
      <div class={styles['box']}>
        <div class={styles['box-item']}>
          <span class={styles['label']}>COUNT</span>
          &nbsp;
          {count}
        </div>
        <div class={styles['box-item']}>
          <span class={styles['label']}>OUTER COUNT</span>
          &nbsp;
          {outerCount}
        </div>
      </div>
      <div class={styles['box']}>
        <div class={styles['box-item']}>
          <span class={styles['label']}>STATIC SLOT</span>
          &nbsp;
          {footer}
        </div>
        <div class={styles['box-item']}>
          <span class={styles['label']}>DYNAMIC SLOT</span>
          &nbsp;
          {renderTime(time)}
        </div>
      </div>
    </div>
  </div>
);
</script>

<script lang="tsx">
export default { name: 'Clock' };
</script>

<template>
  <JSX />
</template>
