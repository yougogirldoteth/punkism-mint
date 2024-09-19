<template>
  <MintToken
    :token="token"
    :mintCount="mintCount"
    #default="{
      displayPrice,
      dollarPrice,
      mintRequest,
      minted,
      mintOpen,
      blocksRemaining,
      transactionFlowConfig
    }"
  >
    <article class="token-overview-card">
      <div class="content">
        <div class="image-container">
          <Image :src="token.artifact" :alt="token.name" />
          <header class="overlay" @click="navigateToTokenPage">
            <IconCheck class="icon-check"/>
              <MintTokenButton
                v-if="mintOpen"
                v-model:mintCount="mintCount"
                v-bind="{
                  token,
                  displayPrice,
                  dollarPrice,
                  mintRequest,
                  transactionFlowConfig,
                  minted,
                }"
                class="mint-token-button"
                @click.stop
              />
            <div class="text-info">
              <h3>
                <span>{{ token.name }} <span class="token-id">#{{ token.tokenId }}</span></span>
              </h3>
              <p v-if="mintOpen" class="closes-in">Closes in {{ blocksRemaining }} {{ pluralize('block', Number(blocksRemaining))}}</p>
              <p v-else class="closed-at">Closed at block {{ token.untilBlock }}</p>
            </div>
          </header>
        </div>
      </div>
    </article>
  </MintToken>
</template>

<script setup lang="ts">
const { token } = defineProps<{
  token: Token
}>()

const store = useOnchainStore()
const collection = computed(() => store.collection(token.collection)) as ComputedRef<Collection>

const mintCount = ref('1')

const navigateToTokenPage = () => {
  const router = useRouter()
  router.push({
    name: 'id-collection-tokenId',
    params: { id: collection.value.owner, collection: token.collection, tokenId: `${token.tokenId}` }
  })
}
</script>

<style scoped>
.token-overview-card,
.token-overview-card > .content {
  display: flex;
  flex-direction: column;
  justify-content: center;
}

.token-overview-card {
  > p {
    color: var(--);
    font-size: var(--font-sm);
    text-align: left;
  }
}

.token-overview-card > .content {
  display: flex;
  flex-direction: column;
  justify-content: center;

  .image-container {
    position: relative;
    width: 100%;
  }

  .image-container img {
    width: 100%;
    height: auto;
  }

  .overlay {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background: rgba(0, 0, 0, 0.5);
    color: white;
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    opacity: 0;
    transition: opacity 0.3s ease;
    padding: var(--spacer);
    box-sizing: border-box;
    z-index: 2;
    cursor: pointer;
    opacity: 0;
    transition: opacity 0.3s ease;
    padding: var(--spacer);
    box-sizing: border-box;
    z-index: 1;

    >svg,
    i {
        width: var(--size-5);
        height: var(--size-5);
        margin: 0;
        color: var(--white);
        transition: all var(--speed);
    }
  }

  .button-container {
    position: absolute;
    top: var(--space-xs);
    right: var(--space-xs);
  }

  .text-info {
    align-self: flex-start;
    margin-top: auto;

    h3,
    p {
        margin: 0;
    }
  }

  .icon-check {
    align-self: flex-start;
  }

  .image-container:hover .overlay {
    opacity: 1;
  }

  header {
    display: flex;
    flex-direction: column;
    gap: var(--spacer-xs);

    @media (--sm) {
      flex-direction: row;
      justify-content: space-between;
      align-items: center;
      gap: var(--spacer);
    }
  }

  h1 {
    span:last-of-type:not(:first-child) {
      color: var(--muted);
      font-size: var(--font-sm);
      white-space: wrap;
      display: none;

      @media (--md) {
        display: block;
      }
    }

    + p {
      @media (--md) {
        text-align: right;
      }
    }
  }
}

footer {
  width: 100%;
  display: flex;
  flex-direction: column;
  gap: var(--spacer);
  justify-content: space-between;

  > * {
    width: 100%;
  }

  > *:first-child:last-child {
    margin-left: auto;
  }
}

.mint-token-button {
  position: absolute;
  top: var(--size-5);
  right: var(--size-5);
  z-index: 3;
}

.hidden-link {
  display: none;
}
</style>
