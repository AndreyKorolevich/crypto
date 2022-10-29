<template>
  <section id="join" class="container mb-24">
      <div class="join-content text-white flex flex-col items-center justify-center text-center relative">
        <img alt="coin_img" class="join-content__btc w-20 h-20 absolute top-10 left-24" src="../images/hero/bitcoin.png" />
        <img alt="coin_img" class="join-content__eth w-20 h-20 absolute top-10 right-24" src="../images/hero/ethereum.png" />
        <div class="join-content__text">
          <h2 class="text-white text-5xl md:text-6xl lg:text-8xl xl:text-4.5xl 2xl:text-3.5xl uppercase leading-tight mb-8 font-bold">
            join us via <br /> <span>discord</span>
          </h2>
          <p class="text-2xl mb-28">Invest and manage all your crypto at one place.</p>
          <a rel="noreferrer" target="_blank" href="https://discord.com/" class="text-lg font-semibold py-6 px-12 text-white rounded-full transition-all duration-200">
            Join via Discord
          </a>
        </div>
      </div>
  </section>
</template>

<script>
export default {
  name: 'Join',
};
</script>