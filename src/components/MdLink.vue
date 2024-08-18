<template>
  <q-item clickable tag="a" target="_blank" @click="changeMd(address)">
    <q-item-section v-if="icon" avatar>
      <q-icon :name="icon" />
    </q-item-section>

    <q-item-section>
      <q-item-label>{{ author }}</q-item-label>
    </q-item-section>
  </q-item>
</template>

<script setup lang="ts">
defineOptions({
  name: 'MdLink'
});

export interface IMdLink {
  author: string;
  address: string;
  icon?: string;
};

defineProps(['author', 'address', 'icon', 'changeMd']);
</script>
