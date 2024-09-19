<template>
  <Button :to="`https://app.ens.domains/${address}`">
    <span>Edit Profile</span>
  </Button>
</template>

<script setup>
defineProps(['address'])
</script>
