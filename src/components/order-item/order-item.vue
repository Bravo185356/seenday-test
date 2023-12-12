<template>
  <StatusCard :status="status">
    <div>№: {{ order.id }}</div>
  </StatusCard>
</template>
<script setup>
import { StatusCard } from "@/ui/status-card/index.ts";

const props = defineProps({
  order: Object
});

const statuses = ref(["green", "blue", "red"]);

const status = computed(() => {
  let index = props.order.status - 1;
  if (props.order.status === "7") {
    index = 2;
  }
  return statuses.value[index];
});
</script>
<style></style>
