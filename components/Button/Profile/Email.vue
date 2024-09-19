<template>
  <div class="category" v-if="profile.email">
    <span class="title">EMAIL</span>
    <a :href="`mailto:${profile.email}`" class="external-link">
      <span>{{ profile.email }}</span>
    </a>
  </div>
</template>

<script setup>
defineProps(['profile'])
</script>

<style>
.category{
  display: flex;
  align-items: center;
  gap: var(--size-2);
}

.title {
  display: inline-flex;
  align-items: center;
  min-width: 100px;
  flex-shrink: 0;
  text-align: left;
  font-size: var(--font-xs);
  background-color: var(--gray-z-9);
  border: none;
  color: var(--half-dark);
  align-self: flex-start;
  padding: var(--size-2) var(--size-2);
}

.external-link {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  border: none;
  cursor: pointer;
  transition: all var(--speed);
}

.external-link:hover {
  color: var(--cp-pink);
}
</style>
