<template>
  <!-- todo: data lines, scrubber -->
  <div class="container flex center" ref="line-container">
    <div class="line" v-for="i in 40"></div>
  </div>
</template>

<script setup lang="ts">
import { onMounted, useTemplateRef } from 'vue';

const OFFSET = 50; // px offset to cursor (abs)
const ref = useTemplateRef('line-container');

onMounted(() => {
  ref.value!.onmouseover = (ev) => {
    const mouseX = ev.clientX;
    for (const c of ref.value!.children) {
      const child = c as HTMLElement;
      const bounds = child.getBoundingClientRect();
      const offset = Math.abs(bounds.x - mouseX);

      const width = Math.max(1, 5 * (1 - (offset / OFFSET)));
      child.style.width = `${width}px`;
      child.style.marginLeft = `${5 - width}px`;

      if (offset <= OFFSET) {
        child.style.backgroundColor = 'var(--o)';
      } else {
        child.style.backgroundColor = 'var(--b)';
      }
    }
  };

  ref.value!.onmouseleave = (ev) => {
    for (const c of ref.value!.children) {
      const child = c as HTMLElement;
      child.style.width = '1px';
      child.style.marginLeft = '4px';
      child.style.backgroundColor = 'var(--b)';
    }
  };
});

</script>

<style scoped>
.container {
  height: 100px;
  aspect-ratio: 3/1;
}

.line {
  position: relative;
  height: 100%;
  width: 1px;
  background-color: var(--b);
  /* transition: 0.05s; */
  margin-left: 4px;
}
</style>