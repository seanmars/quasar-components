<template>
  <q-virtual-scroll
    style="max-height: 450px;"
    bordered
    separator
    :items="props.data"
    v-slot="{ item, index }"
  >
    <q-item
      :active="item.activate"
      clickable
      v-ripple
      active-class="bg-teal-1"
      @click="(e) => handleItemClick(e, item)"
      @dblclick="(e) => handleItemDbClick(e, item)"
    >
      <q-item-section>
        <q-item-label>{{ item.label }}</q-item-label>
      </q-item-section>
    </q-item>
  </q-virtual-scroll>
</template>

<script setup lang="ts">
import { defineProps } from 'vue';

export interface SelectionItemData {
  label: String;
  id: Number;
  activate: boolean;
}

const clickTimeout = 100;
let clickTimeoutId = null;
let clickCount = 0;

const props = defineProps<{
  data: Array<SelectionItemData>,
  onItemClick?: (item: SelectionItemData) => void,
  onItemDbClick?: (item: SelectionItemData) => void,
}>();

const handleItemClick = (e: Event, item: SelectionItemData) => {
  clickCount++;
  if (clickCount === 1) {
    clickTimeoutId = setTimeout(() => {
      props.onItemClick?.(item);
      clickCount = 0;
    }, clickTimeout);//tolerance in ms
  } else {
    clearTimeout(clickTimeoutId);
    clickCount = 0;
  }
};

const handleItemDbClick = (e: Event, item: SelectionItemData) => {
  clearTimeout(clickTimeoutId);
  props.onItemDbClick?.(item);
};

</script>

<style scoped lang="scss">

</style>
