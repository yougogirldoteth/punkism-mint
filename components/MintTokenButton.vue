<template>
  <div v-if="! isConnected" class="connect-button">Connect To Mint</div>
  <FormGroup v-else ref="el">
    <TransactionFlow
      :request="mintRequest"
      :text="transactionFlowConfig"
      @complete="onMinted"
      skip-confirmation
      auto-close-success
    >
      <template #start="{ start }">
        <Button @click="start" class="mint custom-mint-button">
          Mint
        </Button>
      </template>
    </TransactionFlow>
  </FormGroup>
</template>

<script setup>
import { useAccount } from '@wagmi/vue'

const { isConnected } = useAccount()

const props = defineProps({
  token: Object,
  displayPrice: Object,
  dollarPrice: String,
  mintRequest: Function,
  transactionFlowConfig: Object,
  minted: Function,
})

const mintCount = defineModel('mintCount', { default: '1' })
const onMinted = () => {
  mintCount.value = '1'
  props.minted()
}
</script>

<style scoped>
.custom-mint-button {
  color: var(--white);
  background-color: transparent;
  border: 1px solid var(--white);
  font-size: var(--font-sm);
  width: auto;
  position: absolute;
  top: -7px;
  right: -7px;
  padding: var(--size-2) var(--size-2);

  &:hover {
    background-color: transparent !important;
    border-color: var(--cp-pink);
  }
}
</style>
