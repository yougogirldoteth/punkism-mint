<template>
  <div class="breadcrumb">
    <span v-for="item in items">
      <NuxtLink v-if="item.to" :to="item.to">{{ item.text }}</NuxtLink>
      <template v-else>{{ item.text }}</template>
    </span>
  </div>
</template>

<script setup>
defineProps({
  items: Array,
})
</script>

<style scoped>
.breadcrumb {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: var(--spacer-sm);
  color: var(--cp-pink);
  font-weight: var(--font-weight-bold);

  > span {
    white-space: nowrap;
    color: var(--cp-pink);

    &:has(+ span > a) {
      display: none;

      @media (--md) {
        display: inherit;
      }
    }
  }

  > span > a {
    color: var(--gray-z-5);
    transition: all var(--speed);

    &.router-link-active-exact,
    &:--highlight {
      color: var(--cp-pink);
    }
  }

  > span:not(:last-child):after {
    content: '/';
    color: var(--);
    margin-left: var(--spacer-sm);
  }
}
</style>
