<template>
  <slot :collection="collection">
    <CollectionIntro :collection="collection" />
    <div class="collection-gallery">
      <TokenOverviewCard v-for="token of tokens" :key="token.tokenId" :token="token" />
    </div>
    <Loading v-if="loading" />
    <div v-if="! tokens.length && !loading" >
      <p>No tokens yet</p>
    </div>
  </slot>
</template>

<script setup>
const props = defineProps(['collection'])
const collection = computed(() => props.collection)

const store = useOnchainStore()

const tokens = computed(() => store.tokens(collection.value.address))
const loading = ref(false)
onMounted(async () => {
  loading.value = true
  await store.fetchCollectionTokens(collection.value.address)
  loading.value = false
})
</script>


<style scoped>
.collection-gallery {
  --image-width: 15rem;
  --gap: var(--size-4);
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(var(--image-width), 1fr));
  grid-auto-flow: dense;
  gap: var(--gap);
  align-items: center;

  >*:nth-child(4n) {
    grid-column: span 2;
    grid-row: span 2;
  }

  @media (--sm) {
    --image-width: 12rem;
  }

  @media (--md) {
    --image-width: 15rem;
  }

  @media (--lg) {
    --gap: var(--size-5);
  }
}
</style>
