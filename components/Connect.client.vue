<template>
  <Button 
    v-if="showConnect" 
    @click="chooseModalOpen = true" 
    :class="class"
    class="connect-button"
  >
    <slot>Connect</slot>
  </Button>
  <slot v-else name="connected">
    <Account :address="address" />
  </slot>

  <Teleport to="body">
    <Modal
      v-if="showConnect"
      :open="chooseModalOpen"
      @close="closeModal"
      class="connect-modal"
    >
      <div class="wallet-options">
        <Button
          v-for="connector in shownConnectors"
          @click="() => login(connector)"
          class="choose-connector-button"
        >
          <img v-if="ICONS[connector.name]" :src="connector.icon || `/icons/wallets/${ICONS[connector.name]}`" alt="">
          {{ connector.name }}
        </Button>
      </div>
    </Modal>
  </Teleport>
</template>

<script setup>
import { useAccount, useConnect, useChainId } from '@wagmi/vue'

const ICONS = {
  'Coinbase Wallet': 'coinbase.svg',
  'MetaMask': 'metamask.svg',
  'WalletConnect': 'walletconnect.svg',
  'Rainbow': 'rainbow.svg',
}

const props = defineProps(['class'])
const emit = defineEmits(['connected', 'disconnected'])

const chainId = useChainId()
const { connectors, connect } = useConnect()
const { address, isConnected } = useAccount()

const showConnect = computed(() => !isConnected.value)
const shownConnectors = computed(() => {
  const unique = Array.from(
    new Map(
      connectors.map(connector => [connector.name, connector])
    ).values()
  )

  return unique.length > 1
    ? unique.filter(c => c.id !== 'injected')
    : unique
})

const chooseModalOpen = ref(false)
const closeModal = () => {
  chooseModalOpen.value = false
}
const login = async (connector) => {
  connect({ connector, chainId })

  setTimeout(() => {
    chooseModalOpen.value = false
  }, 100)
}

const check = () => isConnected.value ? emit('connected', { address: address.value }) : emit('disconnected')
watch(isConnected, () => check())
onMounted(() => check())
</script>

<style scoped>
.wallet-options {
  display: flex;
  gap: var(--spacer);
  flex-wrap: wrap;
  justify-content: center;
  margin: 1rem;
}
.choose-connector-button {
  img {
    width: var(--size-5);
    display: inline;
  }
}
.connect-button {
  max-width: 10rem;
}

.connect-modal :deep(i) {
  border: 3px solid red !important;
  transition: color 0.3s ease;
}

.connect-modal :deep(i:hover) {
  color: var(--cp-pink);
}
</style>

<style>
/* Global styles */
body > .connect-modal i {
  transition: color 0.3s ease;
  color: var(--gray-z-4);
}

body > .connect-modal i:hover {
  color: var(--cp-pink);
}
</style>
