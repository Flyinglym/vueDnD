<script lang="ts" setup>
  import { useDrag } from "vue3-dnd"
  import { ItemTypes } from "../../utils/dndItemType"
  import { toRefs } from "@vueuse/core"

  const props = defineProps<{
    id: any
    left: number
    top: number
    hideSourceOnDrag?: boolean
  }>()

  const [collect, drag] = useDrag(() => ({
    type: ItemTypes.BOX,
    item: { id: props.id, left: props.left, top: props.top },
    collect: (monitor) => ({
      isDragging: monitor.isDragging(),
    }),
  }))
  const { isDragging } = toRefs(collect)
</script>

<template>
  <div v-if="isDragging && hideSourceOnDrag" :ref="drag" />
  <div v-else :ref="drag" class="box" :style="{ left: `${left}px`, top: `${top}px` }" role="Box" data-testid="box">
    <slot></slot>
  </div>
</template>

<style scoped>
  .box {
    position: absolute;
    padding: 0.5rem 1rem;
    background-color: white;
    border: 1px dashed gray;
    cursor: move;
  }
</style>
