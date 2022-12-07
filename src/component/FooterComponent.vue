<template>
  <footer
    class="footer-content w-full h-96 bg-cover bg-center flex flex-col justify-center items-center text-center"
    style="background-image: url('/src/images/footer/footer-bg.png')"
  >
    <div class="flex flex-col gap-y-6 w-full mx-auto text-center items-center">
      <div class="flex gap-12 text-white text-4xl">
        <i class="fab fa-twitter cursor-pointer"></i>
        <i class="fab fa-discord cursor-pointer"></i>
        <i class="fab fa-facebook cursor-pointer"></i>
        <i class="fab fa-youtube cursor-pointer"></i>
      </div>
      <div class="footer-content__text text-white flex gap-12 mt-3">
        <p class="cursor-pointer">Privacy</p>
        <p class="cursor-pointer">Terms of Use</p>
      </div>
    </div>
  </footer>
</template>

<script>
export default {
  name: 'FooterComponent',
}
</script>
