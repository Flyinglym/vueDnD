<template>
  <div :ref="drag" role="Box" :style="{ ...style, opacity }" :data-testid="`box-${name}`">
    {{ name }}
  </div>
</template>

<script lang="ts" setup>
  import { useDrag } from "vue3-dnd"
  import { ItemTypes } from "../../utils/dndItemType"
  import { computed, unref } from "vue"
  import { toRefs } from "@vueuse/core"

  interface DropResult {
    name: string
  }

  const props = defineProps<{ name: string }>()

  const style = {
    border: "1px dashed gray",
    backgroundColor: "white",
    padding: "0.5rem 1rem",
    marginRight: "1.5rem",
    marginBottom: "1.5rem",
    cursor: "move",
    float: "left",
  }

  //  box作为拖拽源
  const [collect, drag] = useDrag(() => ({
    type: ItemTypes.BOX,
    item: () => ({
      name: props.name,
    }),
    end: (item, monitor) => {
      console.log("end", monitor)

      const dropResult = monitor.getDropResult<DropResult>()
      if (item && dropResult) {
        console.log(item, dropResult)

        alert(`成功将 ${item.name} 放入 ${dropResult.name}!`)
      }
    },
    collect: (monitor) => ({
      isDragging: monitor.isDragging(),
      handlerId: monitor.getHandlerId(),
    }),
  }))

  const { isDragging } = toRefs(collect)

  const opacity = computed(() => (unref(isDragging) ? 0.4 : 1))
</script>

<style scoped>
  .box {
    float: left;
    margin-right: 1.5rem;
    margin-bottom: 1.5rem;
    padding: 0.5rem 1rem;
    background-color: white;
    border: 1px solid gray;
    cursor: move;
  }
  .box .dragging {
    opacity: 0.4;
  }
</style>
