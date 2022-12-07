<template>
  <section class="w-full">
    <div class="flex w-full">
      <div class="w-full">
        <label for="wallet" class="block text-xl font-medium text-white w-full">Ticker</label>
        <div class="relative rounded-md shadow-md w-full">
          <input
            id="wallet"
            v-model="ticker"
            type="text"
            name="wallet"
            class="block p-3 pr-10 w-full text-white focus:outline-none focus:ring-white focus:border-white bg-opacity-5 rounded-xl bg-white border border-opacity-25 border-gray-500"
            placeholder="e.g. BTC"
            @keydown.enter="add"
          />
        </div>
        <div
          v-if="ticker && autocompleteTicker.length"
          class="flex bg-white shadow-md p-1 rounded-md shadow-md flex-wrap"
        >
          <span
            v-for="t of autocompleteTicker"
            :key="t"
            class="inline-flex items-center px-2 m-1 rounded-md text-xs font-medium bg-gray-300 text-gray-800 cursor-pointer"
            @click="
              () => {
                ticker = t
                add()
              }
            "
          >
            {{ t }}
          </span>
        </div>
        <div v-if="isOld">
          <div class="text-sm text-red-600">This ticker has already been added</div>
        </div>
      </div>
    </div>
    <add-button type="button" @click="add" :disabled="disabled" />
  </section>
</template>

<script>
import AddButton from './AddButton.vue'

export default {
  name: 'AddTicker',
  components: {
    AddButton,
  },

  props: {
    oftenTickers: {
      type: Array,
      default() {
        return []
      },
    },

    tickers: {
      type: Array,
      default() {
        return []
      },
    },
  },

  emits: {
    addTicker: (payload) => typeof payload === 'object' && payload.name?.length,
  },

  data() {
    return {
      ticker: '',
      disabled: true,
    }
  },

  computed: {
    autocompleteTicker() {
      return this.oftenTickers?.filter((t) => t.includes(this.ticker?.toUpperCase())).slice(0, 4)
    },

    isOld() {
      return this.tickers?.filter((t) => t.name === this.ticker?.toUpperCase()).length
    },
  },

  watch: {
    ticker() {
      if (!this.ticker || this.isOld) {
        this.disabled = true
        return
      }
      this.disabled = false
    },
  },

  methods: {
    add() {
      if (!this.ticker) {
        this.ticker = ''
        return
      }
      if (this.isOld) {
        return
      }
      const uppercaseTicker = this.ticker.toUpperCase()
      const bgColor = this.oftenTickers.includes(uppercaseTicker) ? 'bg-white' : 'bg-red-100'
      const ticker = {
        name: uppercaseTicker,
        price: '-',
        intervalId: null,
        bgColor,
      }
      this.$emit('addTicker', ticker)
      this.ticker = ''
    },
  },
}
</script>
