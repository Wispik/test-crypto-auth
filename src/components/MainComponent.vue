<template>
  <button
    v-if="!metamaskAddress"
    :disabled="!metamaskProvider"
    @click="authMetamask"
  >
    Auth Metamask
  </button>
  <span v-if="!metamaskProvider">Metamask extension not install</span>
  <span v-if="metamaskAddress">Metamask adress: {{ metamaskAddress }}</span>
  <button 
    v-if="!phantomAddress"
    :disabled="!phantomProvider"
    @click="authPhantom"
  >
    Auth Phantom
  </button>
  <span v-if="!phantomProvider">Phantom extension not install</span>
  <span v-if="phantomAddress">Phantom address: {{ phantomAddress }}</span>
  <button
    v-if="!coinbaseAddress"
    :disabled="!coinbaseProvider"
    @click="authCoinbase"
  >
    Auth Coinbase
  </button>
  <span v-if="!coinbaseProvider">Coinbase extension not install</span>
  <span v-if="coinbaseAddress">Coinbase address: {{ coinbaseAddress }}</span>
</template>

<script>
export default {
  data() {
    return {
      phantomProvider: null,
      metamaskProvider: null,
      coinbaseProvider: null,
      phantomAddress: '',
      metamaskAddress: '',
      coinbaseAddress: ''
    }
  },
  mounted() {
    this.phantomProvider = window.phantom?.solana
    if (window.ethereum) {
      let eth = window.ethereum
      if (eth.providers?.length) {
        eth.providers.forEach (el => {
          if (el.isMetaMask) this.metamaskProvider = el
          if (el.isCoinbaseWallet) this.coinbaseProvider = el
        })
      } else {
        if (eth.isMetaMask) this.metamaskProvider = eth
        if (eth.isCoinbaseWallet) this.coinbaseProvider = eth
      }
    }
  },
  methods: {
    async authMetamask() {
      const accounts = await this.metamaskProvider.request({ method: 'eth_requestAccounts' })
      this.metamaskAddress = accounts[0]
    },
    async authPhantom() {
      const resp = await this.phantomProvider.connect()
      this.phantomAddress = resp.publicKey.toString()
    },
    async authCoinbase() {
      const accounts = await this.coinbaseProvider.request({ method: 'eth_requestAccounts' })
      this.coinbaseAddress = accounts[0]
    }
  }
}
</script>

<style>

</style>