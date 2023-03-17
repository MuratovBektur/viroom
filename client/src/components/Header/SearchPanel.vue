<template>
  <div class="relative">
    <div class="flex">
      <div class="relative w-full">
        <input
          v-model="searchModel"
          class="pl-8 min-h-[3.75rem] bg-[#212121] rounded-full block w-full"
          placeholder="Поиск вдохновения"
        />
        <img
          class="absolute right-5 top-4"
          src="@/assets/search.svg"
          alt="search"
        />
      </div>
      <button
        @click="showSearchCategories = !showSearchCategories"
        class="shrink-0 ml-2.5 h-[3.75rem] w-[3.75rem] rounded-full bg-[#212121] flex items-center justify-center"
      >
        <img src="@/assets/settings.svg" alt="settings" />
      </button>
    </div>
    <SearchCategories
      @on-select-category="onSelectCategory"
      class="absolute bottom-[-3rem]"
      v-show="showSearchCategories"
    />
  </div>
</template>

<script setup lang="ts">
import { ref } from "vue";
import SearchCategories from "./SearchCategories.vue";

const showSearchCategories = ref<boolean>(false);

const searchModel = ref<string>("");

function onSelectCategory(category: string) {
  searchModel.value = category;
}
</script>
