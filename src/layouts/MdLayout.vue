<template>
  <ShowMd :address="address" />
</template>

<script lang="ts" setup name="">
import ShowMd from 'src/components/ShowMd.vue';

defineProps(['address']);
</script>
<style scoped></style>
