<template>
  <NuxtLink v-if="to" :to="to" class="button" :exact="exact" :target="target"><slot /></NuxtLink>
  <button v-else class="button"><slot /></button>
</template>

<script setup>
const { to, exact, gridAligned } = defineProps({
  to: String,
  target: {
    type: String,
    default: '_self',
  },
  exact: Boolean,
})
</script>

<style lang="postcss" scoped>
.button,
button {
  padding: 0.02rem 0.25rem;
  white-space: nowrap;
  background-color: var(--gray-z-9);
  border: none;
  cursor: pointer;
  color: var(--half-dark);
  letter-spacing: var(--letter-spacing);
  text-transform: uppercase;

  &.hover {
    background: var(--gray-z-8);
  }

  &:--highlight,
  &.active {
    background: var(--gray-z-8);
    color: var(--cp-pink);
  }

  &.link.router-link-exact-active,
  &.link.active {
    background: var(--gray-z-4);
  }

  &[disabled]:not([disabled=false]) {
    pointer-events: none;
    color: var(--gray-z-5);
    cursor: normal;
  }

  &.small {
    font-size: var(--font-sm);
    padding: var(--size-2) var(--size-3);
  }

  @media (--md) {
    padding: calc(var(--size-1) + var(--size-2)) var(--size-4);
    height: calc(var(--size-7) + var(--size-1));

    &.small {
      font-size: var(--font-sm);
      padding: var(--size-2) var(--size-3);
      height: calc(var(--size-6) + var(--size-1));
    }
  }

  &.secondary {
    background-color: var(--gray-z-1);
    border-color: var(--gray-z-2);
    color: var(--gray-z-6);

    &:--highlight,
    &.active {
      background: var(--gray-z-3);
      border-color: var(--gray-z-3);
      color: var(--color);
    }
  }
}
</style>