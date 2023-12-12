<template>
  <div class="sort-button-row">
    <Button
      v-for="type in sortTypes"
      :key="type.value"
      :class="{ active: props.currentSortType === type.value }"
      size="small"
      :color="props.currentSortType === type.value ? 'purple' : 'purple-reverse'"
      @click="updateSortType(type.value)"
      ><i v-if="type.icon" :class="type.icon" /><span v-else>{{ type.title }}</span></Button
    >
  </div>
</template>
<script setup>
import { Button } from "@/ui/button/index.ts";

const props = defineProps({
  currentSortType: {
    type: String,
    default: "все"
  }
});

const emit = defineEmits(["update:sortType"]);

const sortTypes = [
  { value: "все", title: "все", icon: "" },
  { value: "sort_type_2", title: "", icon: "fa-regular fa-crown" },
  { value: "sort_type_3", title: "", icon: "fa-regular fa-thumbs-up" },
  { value: "sort_type_4", title: "", icon: "fa-regular fa-thumbs-down" },
  { value: "sort_type_5", title: "", icon: "fa-light fa-briefcase" },
  { value: "sort_type_6", title: "", icon: "fa-light fa-cloud-arrow-up" },
  { value: "sort_type_7", title: "", icon: "fa-light fa-truck" },
  { value: "sort_type_8", title: "", icon: "fa-light fa-id-card-clip" }
];

function updateSortType(sortType) {
  emit("update:sortType", sortType);
}
</script>
<style></style>
