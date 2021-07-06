
<template>
  <h1 class="font-bold text-gray-400 h-full" >
    <font-awesome-icon :icon=faSpinner
      class=" h-full animate-spin" />
  </h1>
</template>


<script >

  import { faSpinner } from '@fortawesome/free-solid-svg-icons'

  export default {

    computed : {
      faSpinner() {return faSpinner}
    },
  }

</script>
