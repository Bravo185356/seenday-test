<template>
  <OrderItem v-if="order" :order="order[0]" />
  <Empty v-else>Заказов ещё нет</Empty>
</template>
<script setup>
import { useRoute } from "vue-router";

import { useAPIFetch } from "@/composables/useAPIFetch.ts";
import { Empty } from "@/ui/empty/index.ts";
import { OrderItem } from "@/components/order-item/index.ts";

const route = useRoute();

const { data } = await useAPIFetch("method/orders.getTest", {
  query: { search_value: route.params.id, search_type: "order_number" }
});
const order = ref(JSON.parse(data.value).response.data.orders);
</script>
<style></style>
