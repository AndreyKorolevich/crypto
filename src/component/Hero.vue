<template>
  <div id="home" class="container">
    <div class="min-h-screen flex flex-col justify-center gap-y-24 pt-16 md:pt-0">
      <div class="text-white max-w-[104rem] text-center uppercase relative mx-auto font-bold">
        <img class="btc-float" src="../images/hero/bitcoin.png" alt="floating-el" />
        <h1 class="text-white text-5xl md:text-6xl lg:text-8xl xl:text-4.5xl 2xl:text-3.5xl">
          Track and Trade
          <br />
          <span class="currencies">Crypto currencies</span>
        </h1>
        <img class="eth-float" src="../images/hero/ethereum.png" alt="floating-el" />
      </div>

      <loading v-if="coinsLoad" />
      <div v-else class="coin-slider grid grid-cols-2 gap-8 md:grid-cols-1">
        <template v-for="item in coins" :key="item.id">
          <div class="slider-coin text-base text-white flex flex-col items-center">
            <img :src="item.image" :alt="item.name" class="w-28 h-28 mb-4" />
            <p class="font-semibold text-xl mb-2">
              {{ item.name }}
              <span :class="`slider-coin__price ${item.price_change_percentage_24h <= 0 ? 'red-text' : 'green-text'}`">
                {{ item.price_change_percentage_24h?.toFixed(2) + '%' }}
              </span>
            </p>
            <p class="font-semibold text-2xl">
              {{ '$ ' + numberWithCommas(item.current_price?.toFixed(2)) }}
            </p>
          </div>
        </template>
      </div>
    </div>
  </div>
</template>

<script>
import Loading from './Loading.vue'
export default {
  name: 'Hero',
  components: {
    Loading,
  },
  data() {
    return {
      coins: [],
      coinsLoad: true,
    }
  },
  async created() {
    const url =
      'https://api.coingecko.com/api/v3/coins/markets?vs_currency=usd&order=market_cap_desc&per_page=4&page=1&sparkline=false'

    try {
      const response = await fetch(url)
      if (!response.ok) {
        throw new Error('Error!')
      }
      this.coins = await response.json()
    } catch (error) {
      console.error(error)
    } finally {
      this.coinsLoad = false
    }
  },
  methods: {
    numberWithCommas(x) {
      return x.toString().replace(/\B(?=(\d{3})+(?!\d))/g, ',')
    },
  },
}
</script>
