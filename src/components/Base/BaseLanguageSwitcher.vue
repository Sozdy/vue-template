<template>
  <div
    class="select"
    @blur="open = false"
  >
    <button
      type="button"
      class="select__selected"
      :class="{ 'select__selected--open': open }"
      :aria-label="$t('locale.ariaOpenList')"
      :aria-expanded="open"
      @click="open = !open"
    >
      {{ Translation.currentLocale }}
      <IconBase
        :stroke="true"
        :width="8"
        :height="4"
        box="0 0 9 5"
      >
        <IconChevron />
      </IconBase>
    </button>
    <div
      class="select__items"
      role="listbox"
      :class="{ 'select__items--open': open }"
    >
      <button
        type="button"
        :id="`lang-${sLocale}`"
        role="option"
        :key="sLocale"
        :aria-selected="sLocale === Translation.currentLocale"
        :aria-label="`${$t('locale.ariaChoose')} ${$t(`locale.${sLocale}`)}`"
        @click="switchLanguage(sLocale), (open = false)"
        v-for="sLocale in supportedLocales"
      >
        {{ sLocale }}
      </button>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref } from "vue"
import { useRouter } from "vue-router"
import Translation from "../../i18n/translation"

import IconBase from "../Icons/IconBase.vue"
import IconChevron from "../Icons/IconChevron.vue"

const open = ref(false)
const supportedLocales = Translation.supportedLocales
const router = useRouter()

const switchLanguage = async (newLocale: string) => {
  await Translation.switchLanguage(newLocale)

  try {
    await router.replace({
      params: { locale: newLocale }
    })
  } catch (e) {
    console.error(e)
    router.push("/")
  }
}
</script>

<style scoped lang="scss">
.select {
  $parent: &;
  position: relative;
  outline: none;
  max-width: 8rem;
  &__selected {
    display: flex;
    align-items: center;
    gap: 0.5rem;
    padding: 0 0.3rem;
    margin: 0 auto;
    font: 400 1.3rem/130% var(--main-font);
    color: var(--color-white);
    text-transform: capitalize;
    opacity: 0.7;
    transition: all 0.2s;
    cursor: pointer;
    &:hover {
      color: var(--color-accent);
      opacity: 1;
    }
    &:focus-visible {
      background-color: var(--color-accent);
      opacity: 1;
    }

    &--open {
      svg {
        transform: rotate(180deg);
      }
    }
  }

  &__items {
    position: absolute;
    background-color: var(--color-black);
    border: 0.1rem solid var(--color-accent);
    top: 2rem;
    left: 0;
    right: 0;
    z-index: 1;
    &:not(&--open) {
      display: none;
    }
    button {
      display: block;
      padding: 0.3rem 0;
      width: 100%;
      font: 400 1.5rem/130% var(--main-font);
      color: var(--color-white);
      text-transform: capitalize;
      cursor: pointer;
      &:hover,
      &:focus-visible {
        background-color: var(--color-accent-hover);
      }
    }
  }
}
</style>
