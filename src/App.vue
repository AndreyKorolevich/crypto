<template>
  <div class="flex flex-col items-center p-4 pb-0 min-h-screen w-full">
    <navbar/>
    <hero />
    <div id="market" class="container mx-auto mb-24">
      <div class="flex items-top gap-x-24 w-full ">
        <add-ticker @add-ticker="add" :often-tickers="oftenTickers" :tickers="tickers" />
        <div class="w-full" v-if="tickers.length">
          <label for="filter" class="block text-xl font-medium text-white"> Filter </label>
          <input
            id="filter"
            v-model="filter"
            type="text"
            placeholder="e.g. ETH"
            class="block p-3 pr-10 w-full text-white focus:outline-none focus:ring-white focus:border-white  bg-opacity-5 rounded-xl bg-white border border-opacity-25 border-gray-500 "
            @keydown.enter="add"
          />
        </div>
      </div>
      <template v-if="tickers.length">
        <div>
          <button
            class="my-4 inline-flex items-center py-2 px-4 border border-transparent shadow-sm text-sm leading-4 font-medium rounded-full text-white bg-gray-600 hover:bg-gray-700 transition-colors duration-300 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-gray-500"
            v-if="page > 1"
            @click="page--"
          >
            back
          </button>
          <button
            class="my-4 mx-2 inline-flex items-center py-2 px-4 border border-transparent shadow-sm text-sm leading-4 font-medium rounded-full text-white bg-gray-600 hover:bg-gray-700 transition-colors duration-300 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-gray-500"
            v-if="hasNextPage"
            @click="page++"
          >
            next
          </button>
        </div>
        <hr class="w-full border-t border-gray-600 my-4" />
        <dl class="mt-5 grid grid-cols-1 gap-5 sm:grid-cols-3">
          <div
            v-for="t of paginatedTickers"
            :key="t"
            :class="[
              {
                'border-4': isTickerSelected(t),
              },
            ]"
            class="ticker overflow-hidden shadow rounded-lg border-white border-solid cursor-pointer"
            @click="select(t)"
          >
            <div class="px-4 py-5 sm:p-6 text-center">
              <dt class="text-sm font-medium text-white truncate">{{ t.name }} - USD</dt>
              <dd class="mt-1 text-3xl font-semibold text-white">
                {{ formatPrice(t.price) }}
              </dd>
            </div>
            <button
              class="flex items-center justify-center font-medium w-full px-4 py-4 sm:px-6 text-md text-white hover:opacity-20 transition-all focus:outline-none"
              @click.stop="remove(t)"
            >
              <svg
                class="h-5 w-5"
                xmlns="http://www.w3.org/2000/svg"
                viewBox="0 0 20 20"
                fill="#ffffff"
                aria-hidden="true"
              >
                <path
                  fill-rule="evenodd"
                  d="M9 2a1 1 0 00-.894.553L7.382 4H4a1 1 0 000 2v10a2 2 0 002 2h8a2 2 0 002-2V6a1 1 0 100-2h-3.382l-.724-1.447A1 1 0 0011 2H9zM7 8a1 1 0 012 0v6a1 1 0 11-2 0V8zm5-1a1 1 0 00-1 1v6a1 1 0 102 0V8a1 1 0 00-1-1z"
                  clip-rule="evenodd"
                ></path>
              </svg>
              Delete
            </button>
          </div>
        </dl>
        <hr class="w-full border-t border-gray-600 my-4" />
        <TheGraph
          v-for="(graph, ticker) of graphs"
          :key="ticker"
          :graph="graph"
          :ticker-name="ticker"
          :selected-ticker="selectedTickers.find((st) => st.name === ticker)"
          @remove-graph="removeGraph(ticker)"
          @update-max-elements="updateMaxElements"
        />
        <loading v-if="isWaitFirstUpdate" />
      </template>
    </div>
    <why-us />
    <join/>
    <footer-component/>
  </div>
</template>

<script>
import { getTickerList, subscribeToTicker, unsubscribeFromTicker } from './api'
import AddTicker from './component/AddTicker.vue'
import TheGraph from './component/TheGraph.vue'
import Loading from './component/Loading.vue'
import Hero from './component/Hero.vue'
import WhyUs from './component/WhyUs.vue'
import Join from './component/Join.vue'
import FooterComponent from './component/FooterComponent.vue'
import Navbar from './component/Navbar.vue'
import { defaultCurrency } from './constants'

export default {
  components: {
    AddTicker,
    TheGraph,
    Loading,
    Hero,
    WhyUs,
    Join,
    FooterComponent,
    Navbar
  },

  data() {
    return {
      oftenTickers: [],
      selectedTickers: [],
      tickers: [],
      graphs: {},
      filter: '',
      page: 1,
      maxGraphElements: 1,
    }
  },

  computed: {
    pageStateOptions() {
      return {
        filter: this.filter,
        page: this.page,
      }
    },

    startIndex() {
      return 6 * (this.page - 1)
    },

    endIndex() {
      return 6 * this.page
    },

    hasNextPage() {
      return this.filteredTickers?.length > this.endIndex
    },

    filteredTickers() {
      return this.tickers?.filter((t) => {
        return t.name?.includes(this.filter?.toUpperCase())
      })
    },

    paginatedTickers() {
      return this.filteredTickers?.slice(this.startIndex, this.endIndex)
    },

    isWaitFirstUpdate() {
      return this.selectedTickers.length !== Object.keys(this.graphs).length
    },

    isTickerSelected() {
      return (ticker) => this.selectedTickers.some((st) => st.name === ticker.name)
    },
  },

  watch: {
    tickers() {
      localStorage.setItem('cryptoexchange', JSON.stringify(this.tickers))
    },

    paginatedTickers() {
      if (!this.paginatedTickers?.length && this.page > 1) {
        this.page -= 1
      }
    },

    selectedTicker() {
      this.$nextTick(this.calculateMaxGraphElements)
    },

    filter() {
      this.page = 1
    },

    pageStateOptions(state) {
      window.history.pushState(null, '', `${window.location.pathname}?filter=${state.filter}&page=${state.page}`)
    },
  },

  async created() {
    this.oftenTickers = [...(await getTickerList())]

    const params = Object.fromEntries(new URL(window.location).searchParams.entries())
    const VALID_KEYS = ['filter', 'page']
    VALID_KEYS.forEach((key) => {
      if (params[key]) {
        this[key] = params[key]
      }
    })

    let tickers = JSON.parse(localStorage.getItem('cryptoexchange'))
    if (!tickers || !tickers.length) {
      tickers = defaultCurrency
    }

      this.tickers = tickers
      this.tickers?.forEach((ticker) => {
        subscribeToTicker(ticker.name, (price) => {
          this.updateTicker(ticker.name, price)
        })
      })

  },

  methods: {
    add(ticker) {
      if (!ticker) {
        return
      }
      this.tickers = [...this.tickers, ticker]
      subscribeToTicker(ticker.name, (price) => {
        this.updateTicker(ticker.name, price)
      })
      this.ticker = ''
    },

    formatPrice(price) {
      if (price === '-') {
        return price
      }
      return price > 1 ? price?.toFixed(2) : price?.toPrecision(3)
    },

    resizeGraph(tickerName) {
      const graph = this.graphs[tickerName]
      if (graph.length > this.maxGraphElements) {
        this.graphs[tickerName] = graph.slice(graph.length - this.maxGraphElements, graph.length)
      }
    },

    updateMaxElements(maxElements, tickerName) {
      this.maxGraphElements = maxElements
      this.resizeGraph(tickerName)
    },

    updateTicker(tickerName, price) {
      this.tickers.find((t) => t.name === tickerName).price = price
      if (this.selectedTickers.some((st) => st.name === tickerName)) {
        if (this.graphs[tickerName]) {
          this.graphs[tickerName] = [...this.graphs[tickerName], price]
        } else {
          this.graphs[tickerName] = [price]
        }
        this.resizeGraph(tickerName)
      }
    },

    removeGraph(tickerName) {
      this.selectedTickers = this.selectedTickers.filter((st) => st.name !== tickerName)
      delete this.graphs[tickerName]
    },

    remove(t) {
      this.tickers = this.tickers.filter((ticker) => {
        if (ticker === t) {
          clearInterval(ticker.intervalId)
          ticker.intervalId = null
        }
        return ticker !== t
      })
      this.removeGraph(t.name)
      unsubscribeFromTicker(t.name)
    },

    select(ticker) {
      this.selectedTickers.push(ticker)
    },
  },
}
</script>
