<template>
  <div>
    <div>
      <q-btn @click="addItem">Add</q-btn>
      <q-btn @click="delItem">Del</q-btn>
    </div>

    <div class="row">
      <div class="col">
        <SelectionList :data="leftData" :on-item-click="onItemClick" :on-item-db-click="onItemDbClick" />
      </div>
      <div class="col">
        <SelectionList :data="rightData" :on-item-click="onItemClick" :on-item-db-click="onItemDbClick" />
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import SelectionList, { SelectionItemData } from './SelectionList.vue';
import { computed, onMounted, ref } from 'vue';

interface ItemData extends SelectionItemData {
  type: number;
}

const data = ref<ItemData[]>([]);

onMounted(() => {
  for (let i = 1; i <= 10; i++) {
    data.value.push({
      label: `item ${i}`,
      id: i,
      activate: false,
      type: 0
    });
  }
});

const leftData = computed<SelectionItemData[]>(() => {
  return data.value.filter(x => x.type === 0);
});

const rightData = computed<SelectionItemData[]>(() => {
  return data.value.filter(x => x.type === 1);
});

const addItem = () => {
  data.value.forEach(x => {
    if (x.activate && x.type === 0) {
      x.type = 1;
      x.activate = false;
    }
  });
};

const delItem = () => {
  data.value.forEach(x => {
    if (x.activate && x.type === 1) {
      x.type = 0;
      x.activate = false;
    }
  });
};

const onItemClick = (item: SelectionItemData) => {
  data.value.forEach(x => {
    if (x.id == item.id) {
      x.activate = !x.activate;
    }
  });
};

const onItemDbClick = (item: SelectionItemData) => {
  data.value.forEach(x => {
    if (x.id === item.id) {
      x.type = x.type === 0 ? 1 : 0;
      x.activate = false;
    }
  });
};
</script>

<style scoped>

</style>