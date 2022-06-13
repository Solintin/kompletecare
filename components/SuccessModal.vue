f
<template>
  <!-- eslint-disable -->

  <section>
    <div
      v-show="isOpen"
      @click="isOpen = !isOpen"
      :class="isOpen ? '  block' : '   hidden'"
      class="px-10 flex justify-center cursor-pointer items-center overlay transform transition duration-300"
    ></div>
    <div
      class="z-10 p-5 success_modal bg-white text-center flex flex-col justify-center items-center space-y-4 rounded-2xl transform transition duration-300"
      :class="isOpen ? '  block' : '   hidden'"
    >
      <div>
        <i
          class="fas fa-check-circle  mx-auto"
        ></i>
      </div>
      <div class="mt-10 mb-4 text-xl text-green-500 font-medium">
        Congratulations!
      </div>
      <h3 class="text-[#201E1ECC] text-sm text-center font-medium">
        {{ message }} with id no: {{ id }}
      </h3>
      <button class="w-full back" @click="isOpen = !isOpen">Go back</button>
    </div>
  </section>
</template>

<script>
/*eslint-disable*/
export default {
  props: ['openModal', 'message', 'id'],
  data() {
    return {
      isOpen: false,
    }
  },
  watch: {
    openModal() {
      this.isOpen = !this.isOpen
    },
  },
}
</script>

<style scoped>
.success_modal {
  position: fixed;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  width: 320px;
}
.overlay {
  position: fixed;
  z-index: 5;
  top: 0;
  right: 0;
  left: 0;
  bottom: 0;
  background: rgba(0, 0, 0, 0.4);
}
.back {
  background: #ed3237;
  padding: 10px 1rem;
  border-radius: 25px;
  color: #fff;
  font-weight: 500;
  outline: none;
  border: 0;
  text-align: center;
  font-size: 12px;
}
</style>
