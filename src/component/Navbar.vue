<template>
  <div>
    <nav class="sticky-nav top-0 right-0 left-0 z-200 bg-black z-40 fixed">
      <div class="flex justify-between max-w-screen-xl mx-auto text-white items-center px-4 py-3">
          <p class="text-3xl font-bold cursor-pointer">CRYPTO</p>
        <ul class="flex gap-24 text-xl list-none hidden md:flex">
          <li>
            <a class="link" href="#home">Home</a>
          </li>
          <li>
            <a class="link" href="#market">Market</a>
          </li>
          <li>
            <a class="link" href="#choose-us">Choose Us</a>
          </li>
          <li>
            <a class="link" href="#join">Join</a>
          </li>
        </ul>
        <span class="flex gap-4">
          <i class="fa-brands fa-twitter link hidden md:flex"></i>
          <i class="fa-brands fa-discord link hidden md:flex"></i>
          <!-- mobile -->
          <i
            @click="openMobile"
            class="fa-solid fa-bars-staggered hamburger-menu"
          ></i>
        </span>
      </div>
    </nav>

    <!-- mobile nav -->
    <div :class="['mobile-nav', mobile ? 'mobile-up' : '']" class="flex flex-col fixed top-0 left-0 w-full h-screen bg-white z-50 text-black text-5xl text-center items-center justify-center transition-all duration-500 ease-in-out">
      <i @click="openMobile" class="fa-solid fa-xmark close-mobile"></i>
      <ul>
        <li @click="openMobile">
          <a href="#home">Home</a>
        </li>
        <li @click="openMobile">
          <a href="#market">Market</a>
        </li>
        <li @click="openMobile">
          <a href="#choose-us">Choose Us</a>
        </li>
        <li @click="openMobile">
          <a href="#join">Join</a>
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
import { ref, onMounted } from 'vue';

export default {
  name: 'Navbar',
  setup() {
    const mobile = ref(false);

    const goTop = () => {
      window.scrollTo({
        top: 0,
        behavior: 'smooth',
      });
    };

    const openMobile = () => {
      mobile.value = !mobile.value;
    };

    return {
      mobile,
      goTop,
      openMobile,
    };
  },
};
</script>
