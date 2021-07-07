<template>
  <main class=" container mx-auto">

    <div class="w-full h-full p-4">
      <h1 class="font-bold text-2xl text-gray-700">Select: </h1>

      <div class="flex gap-2 h-full flex-wrap" >
        <button class="w-full md:w-6/12 lg:w-3/12
          text-left
          shadow-lg
          rounded-lg
          hover:bg-red-50
          p-4
          "
        >

          <div class="w-full flex font-bold">
            <font-awesome-icon :icon=faCoins
              class="text-2xl text-red-500 h-full"/>
            <h1 class="ml-4 text-lg text-gray-800">+50 </h1>
          </div>

          <div class="w-full flex font-light mt-3">

            <h1>P 49.99</h1>
          </div>

        </button>
        <button class="w-full md:w-6/12 lg:w-3/12
          text-left
          shadow-lg
          rounded-lg
          hover:bg-red-50
          p-4
          "
        >

          <div class="w-full flex font-bold">
            <font-awesome-icon :icon=faCoins
              class="text-2xl text-red-500 h-full"/>
            <h1 class="ml-4 text-lg text-gray-800">+100</h1>
          </div>

          <div class="w-full flex font-light mt-3">

            <h1>P 99.99</h1>
          </div>

        </button>

        <button class="w-full md:w-6/12 lg:w-3/12
          text-left
          shadow-lg
          rounded-lg
          hover:bg-red-50
          p-4
          "
          @click=emitCredit
        >

          <div class="w-full flex font-bold">
            <font-awesome-icon :icon=faCoins
              class="text-2xl text-red-500 h-full"/>
            <h1 class="ml-4 text-lg text-gray-800">+210</h1>
          </div>

          <div class="w-full flex font-light mt-3">

            <h1>P 199.99</h1>
          </div>

        </button>

        <button class="w-full md:w-6/12 lg:w-3/12
          text-left
          shadow-lg
          rounded-lg
          hover:bg-red-50
          p-4
          "
        >

          <div class="w-full flex font-bold">
            <font-awesome-icon :icon=faCoins
              class="text-2xl text-red-500 h-full"/>
            <h1 class="ml-4 text-lg text-gray-800">+525</h1>
          </div>

          <div class="w-full flex font-light mt-3">

            <h1>P 500.00</h1>
          </div>

        </button>
      </div>
    </div>

  </main>
</template>

<script>

  import { faCoins } from '@fortawesome/free-solid-svg-icons'

  export default {

    layout: 'app_layout',
    computed :{
      faCoins () { return faCoins }
    },

    methods: {
      emitCredit() {
        console.log('Emitting ')
        this.$parent.$emit('updateCredit', true);

      }

    }
  }
</script>

<style>

</style>
