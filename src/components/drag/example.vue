<script setup lang="ts">
  import Container from "./container.vue"
  import { ref } from "vue"

  const hideSourceOnDrag = ref(true)
  const toggle = () => (hideSourceOnDrag.value = !hideSourceOnDrag.value)
</script>

<template>
  <div>
    <Container :hide-source-on-drag="hideSourceOnDrag" />
    <p>
      <label for="hideSourceOnDrag">
        <input id="hideSourceOnDrag" type="checkbox" role="checkbox" :checked="hideSourceOnDrag" @change="toggle" />
        <small>拖动时隐藏盒子</small>
      </label>
    </p>
  </div>
</template>
