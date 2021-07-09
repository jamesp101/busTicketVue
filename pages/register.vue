<template>
  <main class="container p-3 mx-auto">
    <nuxt-link to="/"
    class="border-b-2 border-solid border-red-200">
       Go back to Login
    </nuxt-link>
    <h1 class="font-bold text-2xl mb-5">Your Account</h1>
    <h1>Basic Information</h1>
    <div class="w-full p-3 flex flex-wrap gap-4">
      <div>
        <p class="text-gray-500 font-light">Firstname</p>
        <input type="text" class="rounded-lg"
          v-model="firstname"  />
      </div>
      <div>
        <p class="text-gray-500 font-light">Lastname</p>
        <input type="text" class="rounded-lg"
          v-model="lastname"  />
      </div>
    </div>
    <div class="w-full p-3 flex flex-wrap gap-4">
      <div>
        <p class="text-gray-500 font-light">Birthdate</p>
        <input  type="date"  class="rounded-lg"
          v-model="birthdate"/>
      </div>
    </div>
    <h1>Address</h1>
    <div class="w-full p-3 flex flex-wrap gap-4">
      <div>
        <p class="text-gray-500 font-light">Street</p>
        <input type="text" class="rounded-lg"
          v-model="street"  />
      </div>
      <div>
        <p class="text-gray-500 font-light">Barangay</p>
        <input type="text" class="rounded-lg"
          v-model="barangay"  />
      </div>
      <div>
        <p class="text-gray-500 font-light">City</p>
        <input type="text" class="rounded-lg"
          v-model="city"  />
      </div>
      <div>
        <p class="text-gray-500 font-light">Province</p>
        <input type="text" class="rounded-lg"
          v-model="province"  />
      </div>
    </div>

    <h1>Contact Information</h1>
    <div class="w-full p-3 flex flex-wrap gap-4">
      <div>
        <div>
          <p class="text-gray-500 font-light">Email</p>
          <input type="text" class="rounded-lg"
            v-model="email"  />
        </div>
      </div>
      <div>
        <p class="text-gray-500 font-light">Phone</p>
        <input type="text" class="rounded-lg"
          v-model="phone"  />
      </div>
    </div>

    <h1>Account Settings</h1>
    <div class="w-full p-3 flex flex-wrap gap-4">
      <div>
        <p class="text-gray-500 font-light">Username</p>
        <input type="text" class="rounded-lg"
          v-model="username"  />
      </div>

      <div >
        <p class="text-gray-500 font-light">Password</p>
        <input type="password" class="rounded-lg"
          v-model="password"  />
      </div>
      <div >
        <p class="text-gray-500 font-light">Confirm Password</p>
        <input type="password" class="rounded-lg"
          v-model="confirm_password"  />
      </div>




    </div>
      <button class="font-bold text-lg
        bg-red-400 text-white p-5 rounded-lg"
        @click=register > Register
      </button>
  </main>

</template>

<script>
  import Swal from 'sweetalert2'
  export default {

    methods: {
      async register(){
        if (this.password != this.confirm_password){
          await Swal.fire({
            icon: 'error',
            text: 'Confirm password is not the same'
          })
        }

        console.log(this.email)
        try{
          await this.$strapi.register({
            username : this.firstname,
            lastname : this.lastname,
            birthdate : this.birthdate,
            street  : this.street,
            barangay : this.barangay,
            city    : this.city,
            province : this.province,
            email  : this.email,
            phone : this.phone,
            username: this.username,
            password: this.confirm_password,
            current_coins: 0
          })

          const confirm = await Swal.fire({
            icon: 'success',
            text: 'Successfully Registered'
          })

          console.log(confirm)
        }catch (e){
          await Swal.fire({
            icon: 'error',
            text: e.message
          })

        }
      }

    },

    data () {
      return {
        firstname: '',
        lastname: '',
        birthdate: '',
        street: '',
        barangay: '',
        city: '',
        province: '',
        email: '',
        phone: '',
        password: '',
        confirm_password: '',
        username: ''

      }
    }

  }
</script>
