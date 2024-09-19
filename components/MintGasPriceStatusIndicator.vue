<template>
    <div class="price-status-container">
      <div class="price-status-indicator">
        <div v-for="i in 6" :key="i" :class="['indicator-line', `line-${i}`, { active: i <= gasPriceLevel }]"></div>
      </div>
      <span class="usd">${{ dollarPrice }}</span>
    </div>
</template>

<script setup>
const props = defineProps({
  mintCount: {
    type: Number,
    default: 1,
  },
})

const { price } = useMintPrice(computed(() => props.mintCount))
const priceFeed = usePriceFeedStore()
const gasPrice = useGasPrice()

const dollarPrice = computed(() => {
  const usd = priceFeed.weiToUSD(price.value)
  return parseFloat(usd) > 0 ? usd : `<0.00`
})

const gasPriceLevel = computed(() => {
  const gweiPrice = parseFloat(gasPrice.value.gwei)
  if (gweiPrice < 5) return 1
  if (gweiPrice < 10) return 2
  if (gweiPrice < 15) return 3
  if (gweiPrice < 30) return 4
  if (gweiPrice < 50) return 5
  return 6
})
</script>

<style scoped>
.price-status-container {
  display: inline-flex;
  align-items: baseline;
  gap: var(--size-2);
}

.price-status-indicator {
  display: inline-flex;
  align-items: flex-end;
  gap: 2px;
  height: 100%;
}

.indicator-line {
  width: 3px;
  background-color: #d8d8d8;
  transition: background-color 0.2s ease;
}

.indicator-line.active {
  background-color: var(--gray-z-5);
}

.price-status-container:hover .indicator-line.active {
  background-color: var(--cp-pink);
}

.line-1 { height: 3px; }
.line-2 { height: 3px; }
.line-3 { height: 3px; }
.line-4 { height: 3px; }
.line-5 { height: 5px; }
.line-6 { height: 10px; }

</style>