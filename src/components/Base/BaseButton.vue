<template>
  <a
    class="btn"
    :href="link"
    :class="`btn--${appearance} ${arrow ? 'btn--arrow' : ''}`"
    v-if="link"
  >
    <slot></slot>
    <span
      v-if="arrow"
      class="btn__arrow"
    >
      <IconBase
        :width="24"
        :height="24"
        box="0 0 24 24"
      >
        <IconArrow />
      </IconBase>
    </span>
  </a>
  <RouterLink
    class="btn"
    :to="to"
    :class="`btn--${appearance} ${arrow ? 'btn--arrow' : ''}`"
    v-else-if="to"
  >
    <slot />
    <span
      v-if="arrow"
      class="btn__arrow"
    >
      <IconBase
        :width="24"
        :height="24"
        box="0 0 24 24"
      >
        <IconArrow />
      </IconBase>
    </span>
  </RouterLink>
  <button
    class="btn"
    :class="`btn--${appearance} ${arrow ? 'btn--arrow' : ''}`"
    :type="type ? type : 'button'"
    v-else
  >
    <slot></slot>
    <span
      v-if="arrow"
      class="btn__arrow"
    >
      <IconBase
        :width="24"
        :height="24"
        box="0 0 24 24"
      >
        <IconArrow />
      </IconBase>
    </span>
  </button>
</template>

<script setup lang="ts">
import type { Button } from "../../interfaces"
import { RouterLink } from "vue-router"
import IconArrow from "../Icons/IconArrow.vue"
import IconBase from "../Icons/IconBase.vue"

withDefaults(defineProps<Button>(), {
  link: undefined,
  type: "button",
  appearance: "primary",
  arrow: false
})
</script>

<style scoped lang="scss">
.btn {
  $parent: &;
  display: inline-flex;
  justify-content: center;
  align-items: center;
  gap: 2rem;
  text-align: center;
  border-radius: 0.8rem;
  font: 600 1.6rem/110% var(--main-font);
  transition: all 0.2s;
  cursor: pointer;
  &:disabled {
    cursor: not-allowed;
  }

  &--arrow {
    padding: 1.4rem 2.4rem;
  }

  &--primary {
    background-color: var(--color-accent);
    color: var(--color-white);
    &:not(#{$parent}--arrow) {
      padding: 1.7rem 2.4rem;
    }
    &:not(:disabled) {
      &:focus-visible {
        background-color: var(--color-accent-hover);
      }
      @media (min-width: 576px) {
        &:hover {
          background-color: var(--color-accent-hover);
        }
      }
    }
  }

  &--white,
  &--gray {
    padding: 1.1rem;
    &:not(:disabled) {
      &:focus-visible {
        background-color: var(--color-accent);
        color: var(--color-white);
      }
      @media (min-width: 576px) {
        &:hover {
          background-color: var(--color-accent);
          color: var(--color-white);
        }
      }
    }
  }

  &--ghost {
    padding: 1.6rem 2.4rem;
    font-weight: 500;
    border: 0.1rem solid var(--color-gray-400);
    color: var(--color-black);
    &:not(:disabled) {
      background-color: var(--color-white);
      &:focus-visible {
        background-color: var(--color-accent);
        color: var(--color-white);
        border-color: var(--color-accent);
      }
      @media (min-width: 576px) {
        &:hover {
          background-color: var(--color-accent);
          color: var(--color-white);
          border-color: var(--color-accent);
        }
      }
    }
    &:disabled {
      background-color: #f7f7f7;
    }
  }

  &--attention {
    padding: 1.6rem;
    font-weight: 500;
    background-color: var(--color-attention);
    color: var(--color-white);
    &:not(:disabled) {
      &:focus-visible {
        background-color: var(--color-attention-hover);
        color: var(--color-white);
      }
      @media (min-width: 576px) {
        &:hover {
          background-color: var(--color-attention-hover);
        }
      }
    }
  }

  &--white {
    background-color: var(--color-white);
    color: var(--color-black);
  }

  &--gray {
    background-color: var(--color-gray-600);
    color: var(--color-black);
  }

  &__arrow {
    width: 2.4rem;
    height: 2.4rem;
  }
}
</style>
