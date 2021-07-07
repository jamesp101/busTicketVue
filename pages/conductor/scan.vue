<template>
  <main class="container">
    <qrcode-stream class="h-1/2 w-1/2" @decode="onDecode"></qrcode-stream>

  </main>

</template>

<script>
  import { QrcodeStream } from 'vue-qrcode-reader'
  import Swal from 'sweetalert2'

  export default {
    layout: 'conductor_layout',
    components: {
      QrcodeStream
    },
    methods :{
      onDecode (code){
        Swal.fire({
          title: 'Error!',
          text: code,
          icon: 'error',
          confirmButtonText: 'Cool'
        })
      }

    }

  }
</script>

<style>

</style>
