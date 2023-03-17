<template>
  <div>
    <div class="relative focus-visible:outline-none">
      <button
        ref="buttonRef"
        class="relative overflow-hidden transition-all duration-400"
        @click="createRipple"
      >
        <img
          :src="card.imgUrl"
          :alt="card.title"
          class="object-cover w-[17.1875rem] xl:w-[41.25rem] h-[15.3125rem] xl:h-[28.125rem] rounded-[0.625rem] xl:rounded-[1.875rem]"
        />
      </button>
      <button
        @click="emit('onClickLike', card.id)"
        class="absolute top-3.5 xl:top-6 right-3.5 xl:right-6 h-9 xl:h-11 w-9 xl:w-11"
      >
        <img v-if="card.isFavorite" src="@/assets/like-favourite.svg" />
        <img v-else src="@/assets/like.svg" />
      </button>
      <div class="hidden xl:block bottom-6 right-6 absolute">
        <img :src="card.locationImgUrl" :alt="card.title" class="w-[7.5rem]" />
      </div>
    </div>
    <div class="hidden xl:block mt-3 max-w-[24rem]">
      <div class="text-xl leading-6 font-bold">
        {{ card.title }}
      </div>
      <div class="mt-3 text-base leading-4.5 font-medium text-white/50">
        {{ card.author }}
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { toRefs, ref } from "vue";
import type { PropType } from "vue";

import type { ICard } from "@/types/index.js";
const props = defineProps({
  card: {
    required: true,
    type: Object as PropType<ICard>,
  },
});
const { card } = toRefs(props);

const emit = defineEmits<{
  (e: "onClickLike", id: number): void;
}>();

const buttonRef = ref<HTMLElement>();

const waveAnimationDuration = ref(500);
const waveAnimationDurationCss = ref(waveAnimationDuration.value + "ms");

function createRipple(event: MouseEvent) {
  const button = event.currentTarget as HTMLElement;

  if (!button) return;

  const circle = document.createElement("div");
  const circleWave = document.createElement("div");

  const diameter = 14;
  const radius = diameter / 2;

  circle.style.width = circle.style.height = `${diameter}px`;
  circle.style.left = `${event.offsetX - radius}px`;
  circle.style.top = `${event.offsetY - radius}px`;

  circleWave.style.width = circleWave.style.height = circle.style.width;
  circleWave.style.left = circle.style.left;
  circleWave.style.top = circle.style.top;

  setTimeout(() => {
    circle.remove();
    circleWave.remove();
  }, waveAnimationDuration.value);

  circle.classList.add("ripple");
  circleWave.classList.add("ripple__wave");

  const ripple = button.getElementsByClassName("ripple")[0];
  const rippleWave = button.getElementsByClassName("ripple__wave")[0];

  if (ripple) {
    ripple.remove();
  }
  if (rippleWave) {
    rippleWave.remove();
  }

  button.appendChild(circle);
  button.appendChild(circleWave);
}
</script>

<style>
div.ripple {
  position: absolute;
  background: white;
  border-radius: 50%;
}

div.ripple__wave {
  position: absolute;
  border-radius: 50%;
  animation: rippleWave v-bind(waveAnimationDurationCss);
  background-color: rgba(255, 255, 255, 0.7);
}

@keyframes rippleWave {
  from {
    transform: scale(0);
  }
  to {
    transform: scale(4.5);
  }
}
</style>
