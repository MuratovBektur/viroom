<template>
  <div>
    <div class="flex justify-between items-center">
      <div
        class="text-[#E2FF1B] text-[2.625em] md:text-[3.25em] xl:text-[5rem] leading-[3.1rem] md:leading-[3.875rem] font-bold"
      >
        Популярные
      </div>
      <div class="flex">
        <span
          class="text-white font-bold text-base sm:text-xl leading-[1.125rem] sm:leading-6"
          >Все <span class="hidden md:inline">популярные</span></span
        >
        <img
          class="ml-2"
          src="@/assets/chevron-right.svg"
          alt="chevron-right"
        />
      </div>
    </div>
    <div class="relative">
      <button
        v-if="showNavigationBtnPrev"
        @click="scrollToLeft"
        class="hidden rotate-180 z-10 xl:flex absolute left-8 top-[11rem] bg-white h-[5rem] w-[5rem] items-center justify-center rounded-full shadow-[0_-12px_14px_rgba(0,0,0,0.137402)]"
      >
        <img class="" src="@/assets/navigation-next.svg" />
      </button>
      <div
        ref="cardListRef"
        v-if="cardList?.length"
        class="scroll-smooth mt-5 flex overflow-auto space-x-8 mb-20 no-scrollbar"
      >
        <Card
          class="flex-nowrap shrink-0"
          @click="toggleFavorite(card.id)"
          v-for="card of cardList"
          :card="card"
          :key="card.id"
        />
      </div>
      <button
        v-if="showNavigationBtnNext"
        @click="scrollToRight"
        class="hidden xl:flex absolute right-14 top-[11rem] bg-white h-[5rem] w-[5rem] items-center justify-center rounded-full shadow-[0_12px_14px_rgba(0,0,0,0.137402)]"
      >
        <img src="@/assets/navigation-next.svg" />
      </button>
    </div>
  </div>
</template>

<script setup lang="ts">
import { onBeforeUnmount, onMounted, ref } from "vue";

import Card from "./Card.vue";
import type { ICard } from "@/types/index.js";

const cardList = ref<Array<ICard>>([
  {
    id: 1,
    isFavorite: false,
    title: "Проект кухни в популярном",
    author: "Камила Магомедовна",
    imgUrl: "https://i.ibb.co/BHpFxnG/image.png",
    locationImgUrl: "https://i.ibb.co/G7NkwvN/image.png",
  },
  {
    id: 2,
    isFavorite: false,
    title: "Проект гостиной в современном стиле",
    author: "Валерия Кононова",
    imgUrl: "https://i.ibb.co/tYMLXc3/image.png",
    locationImgUrl: "https://i.ibb.co/FwLy6Yv/image.png",
  },
  {
    id: 3,
    isFavorite: false,
    title: "Дизайн квартиры-студии",
    author: "Валерия Кононова",
    imgUrl: "https://i.ibb.co/KD2xT7P/image.png",
    locationImgUrl: "https://i.ibb.co/G7NkwvN/image.png",
  },
  {
    id: 4,
    isFavorite: false,
    title: "Проект кухни в популярном",
    author: "Камила Магомедовна",
    imgUrl: "https://i.ibb.co/nDKbN6Y/image.png",
    locationImgUrl: "https://i.ibb.co/FwLy6Yv/image.png",
  },
  {
    id: 5,
    isFavorite: false,
    title: "Дизайн квартиры-студии",
    author: "Валерия Кононова",
    imgUrl: "https://i.ibb.co/KD2xT7P/image.png",
    locationImgUrl: "https://i.ibb.co/G7NkwvN/image.png",
  },
]);

function toggleFavorite(cardId: number) {
  const idx = cardList.value.findIndex((card) => card.id === cardId);
  if (idx !== -1) {
    cardList.value[idx].isFavorite = !cardList.value[idx].isFavorite;
  }
}

const showNavigationBtnNext = ref<boolean>(false);
const showNavigationBtnPrev = ref<boolean>(false);
const cardListRef = ref<HTMLElement>();
const cardWidth = 690;

onMounted(() => {
  onScrollCardList();
  const cardListElem = cardListRef.value;
  if (!cardListElem) return;
  cardListElem.addEventListener("scroll", onScrollCardList);
});

onBeforeUnmount(() => {
  const cardListElem = cardListRef.value;
  if (!cardListElem) return;
  cardListElem.removeEventListener("scroll", onScrollCardList);
});

function onScrollCardList() {
  showNavigationBtnNext.value = !isLastCardVisible();
  showNavigationBtnPrev.value = !isFirstCardVisible();
}

function isFirstCardVisible(): boolean {
  const cardListElem = cardListRef.value;
  if (!cardListElem) return false;
  const firstElem = cardListElem.children[0];
  if (!firstElem) return false;
  return isScrolledIntoView(firstElem as HTMLElement);
}
function isLastCardVisible(): boolean {
  const cardListElem = cardListRef.value;
  if (!cardListElem) return false;
  const lastElem = cardListElem.children[cardListElem.children.length - 1];
  if (!lastElem) return false;
  return isScrolledIntoView(lastElem as HTMLElement);
}

function scrollToRight() {
  if (!cardListRef.value) return;
  cardListRef.value.scrollLeft += cardWidth;
}
function scrollToLeft() {
  if (!cardListRef.value) return;
  cardListRef.value.scrollLeft -= cardWidth;
}

function isScrolledIntoView(el: HTMLElement) {
  const rect = el.getBoundingClientRect();
  const elemLeft = rect.left;
  const elemRight = rect.right;

  // Only completely visible elements return true:
  const isVisible = elemLeft >= 0 && elemRight <= window.innerWidth;
  // Partially visible elements return true:
  //isVisible = elemTop < window.innerHeight && elemBottom >= 0;
  return isVisible;
}
</script>
