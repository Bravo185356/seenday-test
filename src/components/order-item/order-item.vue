<template>
  <StatusCard :status="status">
    <div>№: {{ order.id }}</div>
  </StatusCard>
</template>
<script lang="ts" setup>
import { StatusCard } from "@/ui/status-card/index.ts";
import type { Order } from '@/components/order-list/index.ts';

type Props = {
  order: Order
}

const props = defineProps<Props>();
// Из API приходит число, а в StatusCard нужно передать цвет, чтобы отрисовать статус
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
