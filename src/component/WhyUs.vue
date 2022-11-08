<template>
  <section id="choose-us" class="container mb-44">
    <div class="flex flex-col items-center justify-center text-center">
      <h2 class="text-7xl text-white uppercase mb-40 font-bold">
        why <span class="choose bg-clip-text text-transparent">choose us</span>
      </h2>
      <div class="grid grid-cols-1 text-white text-3xl w-full gap-8 lg:grid-cols-3">
        <div class="flex flex-col gap-12 lg:justify-between">
          <choose-box
            img="fa-solid fa-wallet"
            title="CONNECT YOUR WALLET"
            text="Use Trust Wallet, Metamask or to connect to the app."
          />
          <choose-box
            img="fa-solid fa-pen-ruler"
            title="SELECT YOUR QUANTITY"
            text="Upload your crypto and set a title, description and price."
          />
          <choose-box
            img="fa-solid fa-bolt"
            title="CONFIRM TRANSACTION"
            text="Earn by selling your crypto on our marketplace."
          />
        </div>
        <div class="flex justify-center items-center">
          <img class="w-85 md:w-30 lg:w-45" src="../images/chooseus/choose-main.png" alt="hand_img" />
        </div>
        <div class="flex flex-col gap-12 lg:justify-between">
          <choose-box
            img="fa-solid fa-satellite-dish"
            title="RECEIVE YOUR OWN NFTS"
            text="Invest all your crypto at one place on one platform."
          />
          <choose-box
            img="fa-solid fa-chess-knight"
            title="TAKE A MARKET
              TO SELL"
            text="Discover, collect the right crypto collections to buy or sell."
          />
          <choose-box
            img="fa-solid fa-boxes-stacked"
            title="DRIVE YOUR COLLECTION"
            text="We make it easy to Discover, Invest and manage."
          />
        </div>
      </div>
    </div>
  </section>
</template>

<script>
import ChooseBox from './ChooseBox.vue'

export default {
  components: {
    ChooseBox,
  },
}
</script>

<style scoped>
h2 {
  font-family: 'Noto Sans Georgian', sans-serif;
}
</style>
