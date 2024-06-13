<template>
  <div :ref="setRef" class="card" :style="{ opacity }" :data-handler-id="handlerId">
    {{ text }}
  </div>
</template>
<script setup lang="ts">
  import { computed, ref, unref } from "vue"
  import { useDrag, useDrop } from "vue3-dnd"
  import type { XYCoord, Identifier } from "dnd-core"
  import { ItemTypes } from "../../utils/dndItemType"

  // 由于vue自带的toRefs需要接收响应式对象 所以此处使用vueuse封装的toRefs
  import { toRefs } from "@vueuse/core"

  const props = defineProps<{
    id: any
    text: string
    index: number
    moveCard: (dragIndex: number, hoverIndex: number) => void
  }>()

  interface DragItem {
    index: number
    id: string
    type: string
  }

  const card = ref<HTMLDivElement>()

  /* 放置目标连接到 DnD  */
  const [dropCollect, drop] = useDrop<DragItem, void, { handlerId: Identifier | null }>({
    accept: ItemTypes.CARD,
    // dropCollect返回值
    collect: (monitor) => ({ handlerId: monitor.getHandlerId() }),
    hover(item: DragItem, monitor) {
      // 处理排序
      if (!card.value) {
        return
      }
      const dragIndex = item.index // 被拿起的item
      const hoverIndex = props.index // 即将被替换的item
      // 不要将项目替换为它们自己
      if (dragIndex === hoverIndex) {
        return
      }

      // 确定item的位置
      const hoverBoundingRect = card.value?.getBoundingClientRect()

      // 获得item垂直中间
      const hoverMiddleY = (hoverBoundingRect.bottom - hoverBoundingRect.top) / 2

      // 确定鼠标位置
      const clientOffset = monitor.getClientOffset()

      // 将像素移到顶部
      const hoverClientY = (clientOffset as XYCoord).y - hoverBoundingRect.top

      // 只有当鼠标越过item高度的一半时才执行移动
      // 向下拖动时，仅当光标低于50%时移动
      // 向上拖动时，仅当光标高于50%时移动

      // 向下拖动
      if (dragIndex < hoverIndex && hoverClientY < hoverMiddleY) {
        return
      }

      // 向上拖动
      if (dragIndex > hoverIndex && hoverClientY > hoverMiddleY) {
        return
      }

      // 实际执行操作的时间
      props.moveCard(dragIndex, hoverIndex)

      // 注意：我们正在更改此处的监视器项目！
      // 一般来说，最好避免突变，
      // 但为了表现，这里很好
      // 以避免昂贵的索引搜索
      item.index = hoverIndex
    },
  })

  /* 拖拽源连接到 DnD */
  const [collect, drag] = useDrag({
    type: ItemTypes.CARD,
    item: () => {
      return { id: props.id, index: props.index }
    },

    // collect返回值
    collect: (monitor: any) => ({ isDragging: monitor.isDragging() }),
  })

  const { handlerId } = toRefs(dropCollect)
  const { isDragging } = toRefs(collect)
  const opacity = computed(() => (unref(isDragging) ? 0 : 1)) // 0 为拿起的目标

  const setRef = (el: HTMLDivElement): undefined => {
    card.value = drag(drop(el)) as HTMLDivElement
  }
</script>
<style scoped>
  .card {
    margin-bottom: 0.5rem;
    padding: 0.5rem 1rem;
    background-color: white;
    border: 1px dashed gray;
    cursor: move;
  }
</style>
