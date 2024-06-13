<script lang="ts" setup>
  import { useDrop, XYCoord } from "vue3-dnd"
  import { ItemTypes } from "../../utils/dndItemType"
  import Box from "./box.vue"
  import { reactive } from "vue"

  interface DragItem {
    type: string
    id: string
    top: number
    left: number
  }

  defineProps<{
    hideSourceOnDrag: boolean
  }>()

  const boxes = reactive<{
    [key: string]: {
      top: number
      left: number
      title: string
    }
  }>({
    a: { top: 20, left: 80, title: "盒子一" },
    b: { top: 180, left: 20, title: "盒子二" },
  })

  const moveBox = (id: string, left: number, top: number) => {
    Object.assign(boxes[id], { left, top })
  }

  const [, drop] = useDrop(() => ({
    accept: ItemTypes.BOX,
    drop(item: DragItem, monitor) {
      const delta = monitor.getDifferenceFromInitialOffset() as XYCoord
      const left = Math.round(item.left + delta.x)
      const top = Math.round(item.top + delta.y)
      moveBox(item.id, left, top)
      return undefined
    },
  }))
</script>

<template>
  <div :ref="drop" class="container">
    <Box v-for="(value, key) in boxes" :id="key" :key="key" :left="value.left" :top="value.top" :hide-source-on-drag="hideSourceOnDrag">
      {{ value.title }}
    </Box>
  </div>
</template>

<style scoped>
  .container {
    position: relative;
    width: 300px;
    height: 300px;
    border: 1px solid black;
  }
</style>
