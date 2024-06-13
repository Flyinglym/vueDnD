<template>
  <div class="box">
    <card v-for="(card, index) in data" :id="card.id" :key="card.id" :text="card.text" :index="index" :move-card="moveCard" />
  </div>
</template>
<script setup lang="ts">
  import { ref } from "vue"
  import card from "./card.vue"

  interface Item {
    id: number
    text: string
  }

  const data = ref<Item[]>([
    { id: 1, text: "Write a cool JS library" },
    { id: 2, text: "Make it generic enough" },
    { id: 3, text: "Write README" },
    { id: 4, text: "Create some examples" },
    { id: 5, text: "Spam in Twitter and IRC to promote it (note that this element is taller than the others)" },
    { id: 6, text: "???" },
    { id: 7, text: "PROFIT" },
  ])

  const moveCard = (dragIndex: number, hoverIndex: number) => {
    const item = data.value[dragIndex]
    data.value.splice(dragIndex, 1)
    data.value.splice(hoverIndex, 0, item)
  }
</script>
<style scoped>
  .box {
    width: 500px;
    margin: 0 auto;
  }
</style>
