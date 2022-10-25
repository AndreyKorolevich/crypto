<template>
  <div
    class="rounded-lg bg-opacity-5 rounded-3xl bg-white px-8 py-16 border border-opacity-25 border-gray-500 flex justify-center gap-8 font-small w-full"
  >
    <i :class="img" class="choose-i flex items-center w-16 h-16 p-4 rounded-xl"></i>
    <div>
      <h4 class="text-2xl font-bold uppercase text-left">{{ title }}</h4>
      <p class="text-base text-xl text-left mt-4 text-gray-300">{{ text }}</p>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    img: {
      type: String,
      required: true,
    },
    title: {
      type: String,
      required: true,
    },
    text: {
      type: String,
      required: true,
    },
  },
}
</script>
