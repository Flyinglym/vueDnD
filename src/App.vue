<script setup lang="ts">
  import { ref } from "vue"
  import { Tabs as ATabs, TabPane as ATabPane } from "ant-design-vue"
  import { DndProvider } from "vue3-dnd"
  import { HTML5Backend } from "react-dnd-html5-backend"
  import sortContainer from "./components/sort"
  import wasteContainer from "./components/waste"
  import dragContainer from "./components/drag"
  
  const activeKey = ref("1")
</script>

<template>
  <DndProvider :backend="HTML5Backend">
    <a-tabs v-model="activeKey" style="width: 800px; margin: 0 auto">
      <a-tab-pane key="1" tab="垃圾桶"><wasteContainer /></a-tab-pane>
      <a-tab-pane key="2" tab="排序"><sortContainer /></a-tab-pane>
      <a-tab-pane key="3" tab="拖动"><dragContainer /></a-tab-pane>
    </a-tabs>
  </DndProvider>
</template>
