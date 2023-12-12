<template>
  <div v-if="orders">
    <div v-for="order in orders" :key="order.id" class="block">
      <OrderItem :order="order" />
    </div>
  </div>
  <Empty v-else>Заказов ещё нет</Empty>
</template>
<script setup>
import { useRoute } from "vue-router";
import { stringify } from "qs";

import { OrderItem } from "@/components/order-item/index.ts";
import { Empty } from "@/ui/empty/index.ts";
import { useAPIFetch } from "@/composables/useAPIFetch.ts";

const route = useRoute();

const path = ref("method/orders.getTest");
// Передача query в options, в useAPIFetch, не приводила к рефетчу, когда изменялся route.query. При изменении url происходит рефетч
const url = ref(`${path.value}?${stringify(route.query)}`);
const { data, status } = await useAPIFetch(url);
const orders = ref(JSON.parse(data.value).response.data.orders);

watch(() => route.query, () => {
  url.value = `${path.value}?${stringify(route.query)}`;
});
// Когда заканчивается рефетч, получаем status - success и обновляем orders
watch(() => status.value, () => {
  if (status.value === "success") {
    orders.value = JSON.parse(data.value).response.data.orders;
  }
});
</script>
<style></style>
