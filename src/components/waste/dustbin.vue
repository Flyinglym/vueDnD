<script setup lang="ts">
  import { useDrop } from "vue3-dnd"
  import { ItemTypes } from "../../utils/dndItemType"
  import { computed, unref } from "vue"
  import { toRefs } from "@vueuse/core"

  const style = {
    height: "12rem",
    width: "12rem",
    marginRight: "1.5rem",
    marginBottom: "1.5rem",
    color: "white",
    padding: "1rem",
    textAlign: "center",
    fontSize: "1rem",
    lineHeight: "normal",
    float: "left",
  }

  //  dustbin作为放置目标
  const [collect, drop] = useDrop(() => ({
    accept: ItemTypes.BOX,
    drop: () => ({ name: "Dustbin" }),
    collect: (monitor) => ({
      isOver: monitor.isOver(),
      canDrop: monitor.canDrop(),
    }),
  }))
  const { canDrop, isOver } = toRefs(collect)
  const isActive = computed(() => unref(canDrop) && unref(isOver))
  const backgroundColor = computed(() => (unref(isActive) ? "darkgreen" : unref(canDrop) ? "darkkhaki" : "#222"))
</script>

<template>
  <div :ref="drop" role="Dustbin" :style="{ ...style, backgroundColor }">
    {{ isActive ? "释放以放下" : "将方框拖到此处" }}
  </div>
</template>
