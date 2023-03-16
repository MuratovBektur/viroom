<template>
  <header class="pt-4 pb-[4rem] md:pt-9">
    <div class="flex items-start justify-between md:justify-start">
      <div class="flex items-center cursor-pointer">
        <img src="@/assets/logo.svg" alt="logo" />
        <span class="ml-4 text-2xl leading-2 font-bold">viroom</span>
      </div>
      <SearchPanel class="hidden grow md:block px-5" />
      <button
        class="bg-indigo-600 hover:bg-indigo-500 transition-all duration-300 min-h-[3.75rem] py-4.5 px-9 min-w-[10rem] rounded-full"
      >
        <span class="font-bold text-xl leading-2"> Войти </span>
      </button>
    </div>
    <SearchPanel class="block md:hidden mt-5" />
  </header>
</template>

<script setup lang="ts">
import SearchPanel from "./SearchPanel.vue";
</script>
