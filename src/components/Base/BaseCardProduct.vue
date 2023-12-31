<template>
  <article
    class="card"
    :id="`product-card-${slug}`"
  >
    <div class="card__picture">
      <img
        width="310"
        height="310"
        :src="
          getImageUrl({
            fileName: cover.length > 0 ? `${cover}.jpg` : 'nopic.jpg',
            folder: cover.length > 0 ? `images/products/${category}` : 'images/products'
          })
        "
        :alt="title[Translation.currentLocale]"
      />
      <span
        v-if="attr.newest"
        class="card__attr card__attr--newest"
      >
        {{ $t("product.attr.newest") }}
      </span>
      <span
        v-if="price.discount && !attr.newest"
        class="card__attr card__attr--sales"
      >
        {{ $t("product.attr.sales") }}
      </span>
      <BaseButtonFavorite
        class="card__favorite"
        :slug="slug"
      />
      <RouterLink
        class="card__link"
        :to="Translation.i18nRoute({ name: 'product', params: { slug } })"
        :aria-label="title[Translation.currentLocale]"
        :tabindex="-1"
      />
    </div>

    <RouterLink
      class="card__title"
      :to="Translation.i18nRoute({ name: 'product', params: { slug } })"
      :title="title[Translation.currentLocale]"
    >
      <h3>{{ title[Translation.currentLocale] }}</h3>
    </RouterLink>

    <div class="card__price">
      <span class="card__price-regular"> ${{ getDivisors(price.regular) }} </span>
      <s
        class="card__price-old"
        v-if="price.discount"
      >
        ${{ getPercent(price.regular, price.discount) }}
      </s>
    </div>
    <BaseButtonCart
      class="card__cart"
      :slug="slug"
    />
  </article>
</template>

<script setup lang="ts">
import type { Product } from "../../interfaces"
import { RouterLink } from "vue-router"
import { getImageUrl } from "../../utils/getImageUrl"
import { getDivisors } from "../../utils/getDivisors"
import { getPercent } from "../../utils/getPercent"
import Translation from "../../i18n/translation"

import BaseButtonCart from "./BaseButtonCart.vue"
import BaseButtonFavorite from "./BaseButtonFavorite.vue"

defineProps<Product>()
</script>

<style scoped lang="scss">
.card {
  $parent: &;
  display: grid;
  grid-template-columns: auto 1fr;
  gap: 0 1rem;

  &__picture {
    grid-area: 1 / 1 / 1 / 3;
    position: relative;
    width: 100%;
    margin-bottom: 1.4rem;
    @media (min-width: 769px) {
      height: 25rem;
      &:not(&:hover) {
        #{$parent}__favorite {
          opacity: 0;
        }
      }
    }
    @media (max-width: 768px) {
      height: 20rem;
    }

    img {
      width: 100%;
      height: 100%;
      object-fit: cover;
      object-position: center;
      border: 0.1rem solid var(--color-gray-400);
      border-radius: 0.6rem;
    }
  }

  &__attr,
  &__favorite {
    position: absolute;
    display: flex;
    justify-content: center;
    align-items: center;
    z-index: 2;
  }

  &__attr {
    padding: 0.6rem 1rem;
    text-transform: capitalize;
    font: 500 1.3rem/110% var(--main-font);
    color: var(--color-white);
    border-radius: 0.4rem;
    @media (min-width: 576px) {
      top: 2rem;
      left: 2rem;
    }
    @media (max-width: 575px) {
      left: 1rem;
      bottom: 1rem;
    }
    &--sales {
      background-color: var(--color-delivery);
    }
    &--newest {
      background-color: var(--color-newest);
    }
  }

  &__favorite {
    @media (min-width: 576px) {
      right: 2rem;
      top: 2rem;
    }
    @media (max-width: 575px) {
      right: 1rem;
      top: 1rem;
    }
  }

  &__link {
    position: absolute;
    width: 100%;
    height: 100%;
    top: 0;
    left: 0;
    border-radius: 0.6rem;
    color: var(--color-black);
    z-index: 1;
  }

  &__title {
    grid-area: 2 / 1 / 3 / 2;
    padding: 0 0.3rem;
    margin: 0 -0.3rem 0.5rem;
    text-align: left;
    color: var(--color-black);
    word-wrap: anywhere;
    display: -webkit-box;
    -webkit-line-clamp: 1;
    -webkit-box-orient: vertical;
    overflow: hidden;
    transition: all 0.2s;
    &:focus-visible {
      background-color: var(--color-accent);
      color: var(--color-white);
    }
    @media (min-width: 769px) {
      font: 400 1.6rem/130% var(--main-font);
      &:hover {
        color: var(--color-accent-hover);
      }
    }
    @media (max-width: 768px) {
      font: 400 1.8rem/130% var(--main-font);
    }
  }

  &__price {
    display: flex;
    align-items: center;
    gap: 0.5rem;
    grid-area: 3 / 1 / 4 / 2;
    &-regular {
      font: 600 1.8rem/110% var(--main-font);
      color: var(--color-black);
    }
    &-old {
      margin-left: 0.4rem;
      font: 400 1.4rem/110% var(--main-font);
      color: var(--color-gray-200);
      text-decoration: line-through;
    }
  }

  &__cart {
    grid-area: 2 / 2 / 4 / 3;
    justify-self: flex-end;
    align-self: center;
  }
}
</style>
