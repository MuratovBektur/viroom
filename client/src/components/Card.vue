<template>
  <div class="relative focus-visible:outline-none">
    <img
      :src="card.imgUrl"
      :alt="card.title"
      class="object-cover w-[17.1875rem] xl:w-[41.25rem] h-[15.3125rem] xl:h-[28.125rem] rounded-[0.625rem] xl:rounded-[1.875rem]"
    />
    <button
      class="absolute top-3.5 xl:top-6 right-3.5 xl:right-6 h-9 xl:h-11 w-9 xl:w-11"
    >
      <img v-if="card.isFavorite" src="@/assets/like-favourite.svg" />
      <img v-else src="@/assets/like.svg" />
    </button>
    <div class="hidden xl:block bottom-6 right-6 absolute">
      <img :src="card.locationImgUrl" :alt="card.title" class="w-[7.5rem]" />
    </div>
  </div>
</template>

<script setup lang="ts">
import { toRefs } from "vue";
import type { PropType } from "vue";

import type { ICard } from "@/types/index.js";
const props = defineProps({
  card: {
    required: true,
    type: Object as PropType<ICard>,
  },
});
const { card } = toRefs(props);
</script>
