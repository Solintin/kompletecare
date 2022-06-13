<template>
  <div class="bg-gray-100">
    <div class="grid grid-cols-12 h-screen">
      <div class="col-span-2"><Sidebar /></div>
      <div class="col-span-10 px-16 container mx-auto overflow-y-scroll">
        <Main />
      </div>
    </div>
  </div>
</template>

<script>
import Sidebar from '../components/Sidebar.vue'
import Main from '../components/Main.vue'
export default {
  head() {
    return {
      title: 'Dashboard | Best telemedicine platform in Africa',
    }
  },
  components: { Sidebar, Main },
}
</script>
