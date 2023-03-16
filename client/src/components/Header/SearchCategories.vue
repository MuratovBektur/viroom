<template>
  <div v-if="categories?.length" class="mt-3.5 flex items-center">
    <div class="relative">
      <div class="overflow-auto no-scrollbar">
        <div
          ref="categoriesRef"
          class="flex max-w-[calc(100vw-15rem)] md:max-w-[calc(100vw-40rem)] xl:max-w-[calc(100vw-47rem)] 2xl:max-w-[50rem] space-x-2"
        >
          <button
            v-for="category of sortedCategories"
            @click="emit('onSelectCategory', category.title)"
            :key="category.id"
            class="shrink-0 flex items-center bg-[#212121] rounded-full"
          >
            <img
              class="w-9 h-9 rounded-full mr-2.5"
              :src="category.imgUrl"
              :alt="category.title"
            />
            <span class="mr-3.5 font-semibold text-sm leading-4">
              {{ category.title }}
            </span>
          </button>
        </div>
      </div>
      <div
        class="bg-gradient-to-r from-black/0 to-black absolute w-[4.35rem] h-full top-0 right-0 z-1"
      ></div>
    </div>
    <div class="relative">
      <button
        v-if="categories.length > 1"
        @click="showSortTypeVariants = !showSortTypeVariants"
        class="w-[10rem] flex justify-end items-center ml-8 xl:ml-[8.5rem] text-white/40 font-medium text-sm leading-4"
      >
        <span> {{ selectedSortType.title }}</span>
        <img
          class="block ml-1.5"
          src="@/assets/chevron-bottom.svg"
          alt="chevron"
        />
      </button>
      <div
        class="absolute top-5 right-0 whitespace-nowrap mx-auto border border-white"
        :class="{ block: showSortTypeVariants, hidden: !showSortTypeVariants }"
      >
        <button
          v-for="sortType of sortTypes"
          :key="sortType.title"
          @click="onSelectSortType(sortType)"
          class="block border-b border-white last:border-0 text-left w-full py-1 px-2"
        >
          {{ sortType.title }}
        </button>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { orderBy } from "lodash";
import { ref, computed } from "vue";

interface ISortType {
  title: string;
  value: string;
}

const emit = defineEmits<{
  (e: "onSelectCategory", category: string): void;
}>();

const categories = ref([
  {
    id: 1,
    title: "Детская",
    imgUrl: "https://i.ibb.co/PCtdgZP/image.png",
    viewCount: 1000,
    addedDate: new Date(2020, 10, 12),
  },
  {
    id: 2,
    title: "Гардеробная",
    imgUrl: "https://i.ibb.co/xMVGfFj/image.png",
    viewCount: 2000,
    addedDate: new Date(2020, 9, 12),
  },
  {
    id: 3,
    title: "2x комнатные",
    imgUrl: "https://i.ibb.co/9T5w0yk/image.png",
    viewCount: 3000,
    addedDate: new Date(2020, 7, 12),
  },
  {
    id: 4,
    title: "Прихожая",
    imgUrl: "https://i.ibb.co/YDCGb0Y/image.png",
    viewCount: 4000,
    addedDate: new Date(2020, 6, 12),
  },
  {
    id: 5,
    title: "3x комнатные",
    imgUrl: "https://i.ibb.co/g4rvSrq/rsz-6411c58f2b7b6.png",
    viewCount: 6000,
    addedDate: new Date(2020, 0, 12),
  },
]);

const sortedCategories = computed(() =>
  orderBy(categories.value, [selectedSortType.value.value], ["desc"])
);

const sortTypes = ref<Array<ISortType>>([
  {
    title: "Сначала новые",
    value: "addedDate",
  },
  {
    title: "Сначала популярные",
    value: "viewCount",
  },
]);

const selectedSortType = ref<ISortType>(sortTypes.value[0]);

const showSortTypeVariants = ref<boolean>(false);

function onSelectSortType(sortType: ISortType) {
  selectedSortType.value = sortType;
  showSortTypeVariants.value = false;
}

const categoriesRef = ref<HTMLDivElement>();
</script>
