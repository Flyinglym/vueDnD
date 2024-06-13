<script lang="ts" setup>
  import Dustbin from "./dustbin.vue"
  import Box from "./box.vue"
</script>

<template>
  <div>
    <div :style="{ overflow: 'hidden', clear: 'both' }">
      <!-- 垃圾桶 -->
      <Dustbin />
    </div>
    <div :style="{ overflow: 'hidden', clear: 'both' }">
      <!-- 垃圾 -->
      <Box name="Glass" />
      <Box name="Banana" />
      <Box name="Paper" />
    </div>
  </div>
</template>
