
<template>
  <div class="container mx-auto h-full">
    <div class="w-1/2 h-1/2 flex flex-col gap-10 m-auto" >
      <nuxt-link to="/conductor/scan" class="h-full w-full bg-red-400 text-white grid place-content-center">
        Scan
      </nuxt-link>
      <nuxt-link to="/conductor/history" class="h-full w-full bg-red-400 text-white grid place-content-center">
        History
      </nuxt-link>
      <nuxt-link to="/conductor/Account" class="h-full w-full bg-red-400 text-white grid place-content-center">
        Account
      </nuxt-link>

    </div>

  </div>
</template>

<script>
  export default {
    layout: 'conductor_layout'

  }
</script>

<style>

</style>
