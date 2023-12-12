<template>
  <fixed-left-column>
    <template v-slot:fixed>
      <Button class="back-button" width="full" size="large" color="light-purple-reverse">Назад</Button>
      <div class="block">
        <DateRange :date="date" @change="newDateRange => (date = newDateRange)" />
        <search
          :type="searchType"
          :types="selectItems"
          :search-query="searchInput"
          @update:search-query="newValue => (searchInput = newValue)"
          @update:type="newType => (searchType = newType)"
          @submit="submit"
        />
        <SortTypeButtons :current-sort-type="sortType" @update:sort-type="newSortType => (sortType = newSortType)" />
      </div>
      <div class="block">
        <Select
          :items="years"
          :name="selectedYear"
          :model-value="selectedYear"
          @update:model-value="newYear => (selectedYear = newYear)"
        />
        <div class="control-buttons">
          <Button width="full" size="large" color="purple" @click="submit">Показать</Button>
          <Button width="full" size="large" color="purple-reverse">Сбросить</Button>
        </div>
      </div>
    </template>
    <div class="list-wrapper">
      <NuxtPage v-if="route.params.id" />
      <OrderList v-else />
    </div>
  </fixed-left-column>
</template>
<script setup>
import { useRouter, useRoute } from "vue-router";

import fixedLeftColumn from "@/views/layout/fixed-left-column.vue";
import { DateRange } from "@/ui/date-range/index.ts";
import { Search } from "@/components/shared/search/index.ts";
import { Button } from "@/ui/button/index.ts";
import { unixTimestampToLocale } from "@/lib/unix-timestamp-to-locale.ts";
import { Select } from "@/ui/select/index.ts";
import SortTypeButtons from "@/components/sort-types/index.ts";
import { OrderList } from "@/components/order-list/index.ts";

const router = useRouter();
const route = useRoute();

const searchType = ref("order_number");
const sortType = ref("все");
const searchInput = ref("");
const selectedYear = ref("");
const date = ref(null);
const selectItems = ref({
  order_number: { placeholder: "Введите номер заказа", title: "Номер заказа" },
  psid: { placeholder: "Введите номер фотосессии", title: "Номер фотосессии" },
  client_id: { placeholder: "Введите клиент ID", title: "Клиент ID" },
  phone: { placeholder: "Введите телефон", title: "Телефон" },
  email: { placeholder: "Введите Email", title: "Email" },
  name: { placeholder: "Введите плательщика, ребенка", title: "Имя" }
});
const years = ref([
  { title: "2021", value: "2021" },
  { title: "2022", value: "2022" },
  { title: "2023", value: "2023" }
]);

function submit() {
  const query = {};

  query.search_type = searchType.value;

  if (searchInput.value) {
    query.search_value = searchInput.value;
  }

  if (selectedYear.value) {
    query.year = selectedYear.value;
  }

  if (date.value) {
    query.date_start = unixTimestampToLocale(date.value.date_start, { dateStyle: "short" })
      .split(".")
      .reverse()
      .join("");
    query.date_finish = unixTimestampToLocale(date.value.date_finish, { dateStyle: "short" })
      .split(".")
      .reverse()
      .join("");
  }
  router.push({ path: "/manager/orders", query });
}
</script>
<style>
@import "assets/styles/pages/orders/left-sidebar.scss";
</style>
