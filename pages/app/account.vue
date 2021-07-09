
<template>
  <main class="container mx-auto py-10 overflow-auto">
    <h1 class="font-bold text-2xl mb-5">Your Account</h1>


    <h1>Basic Information</h1>
    <div class="w-full p-3 flex flex-wrap gap-4">
      <div>
        <p class="text-gray-500 font-light">Firstname</p>
        <input type="text" class="rounded-lg"
          v-model="firstname" :disabled="isFormDisabled" />
      </div>
      <div>
        <p class="text-gray-500 font-light">Lastname</p>
        <input type="text" class="rounded-lg"
          v-model="lastname" :disabled="isFormDisabled" />
      </div>
    </div>
    <div class="w-full p-3 flex flex-wrap gap-4">
      <div>
        <p class="text-gray-500 font-light">Birthdate</p>
        <input  type="date"  class="rounded-lg"
          v-model="birthdate" :disabled="isFormDisabled"/>
      </div>
    </div>
    <h1>Address</h1>
    <div class="w-full p-3 flex flex-wrap gap-4">
      <div>
        <p class="text-gray-500 font-light">Street</p>
        <input type="text" class="rounded-lg"
          v-model="street" :disabled="isFormDisabled" />
      </div>
      <div>
        <p class="text-gray-500 font-light">Barangay</p>
        <input type="text" class="rounded-lg"
          v-model="barangay" :disabled="isFormDisabled" />
      </div>
      <div>
        <p class="text-gray-500 font-light">City</p>
        <input type="text" class="rounded-lg"
          v-model="city" :disabled="isFormDisabled" />
      </div>
      <div>
        <p class="text-gray-500 font-light">Province</p>
        <input type="text" class="rounded-lg"
          v-model="province" :disabled="isFormDisabled" />
      </div>
    </div>

    <h1>Contact Information</h1>
    <div class="w-full p-3 flex flex-wrap gap-4">
      <div>
        <div>
          <p class="text-gray-500 font-light">Email</p>
          <input type="text" class="rounded-lg"
            v-model="email" :disabled="isFormDisabled" />
        </div>
      </div>
      <div>
        <p class="text-gray-500 font-light">Phone</p>
        <input type="text" class="rounded-lg"
          v-model="phone" :disabled="isFormDisabled" />
      </div>
    </div>

    <h1>Account Settings</h1>
    <div class="w-full p-3 flex flex-wrap gap-4">
      <div>
        <p class="text-gray-500 font-light">Username</p>
        <input type="text" class="rounded-lg"
          v-model="username" :disabled="isFormDisabled" />
      </div>

      <div v-if="!isFormDisabled">
        <p class="text-gray-500 font-light">Password</p>
        <input type="password" class="rounded-lg"
          v-model="password" :disabled="isFormDisabled" />
      </div>

      <div v-if="!isFormDisabled">
        <p class="text-gray-500 font-light">Confirm Password</p>
        <input type="password" class="rounded-lg"
          v-model="confirm_password" :disabled="isFormDisabled" />
      </div>

    </div>
    <button class="
      bg-red-400 font-bold text-lg text-white
      p-5 rounded-lg"
      v-if="isFormDisabled"
      @click=editMode
    > Edit</button>
    <button class="
      bg-green-400 font-bold text-lg text-white
      p-5 rounded-lg"
      @click=saveEdit
      v-else
    > Finish</button>
  </main>


</template>

<script>
  import Swal from 'sweetalert2'

  export default {
    layout: 'app_layout',

    beforeMount( ){
      this.setUser()
    },

    methods : {
      editMode() {
        this.isFormDisabled = false
      },

      async saveEdit(){

        if(this.password !='' && this.password !=''){
          if(this.password == this.confirm_password){
            Swal.fire({
              text: 'Password is not the same',
              icon: 'error',
            })
            return
          }
        }

        const payload = {
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
        }

        if(!(this.password == '' || this.confirm_password == '')){
          payload.password = this.confirm_password
        }


        console.log(payload)
        await this.$strapi.update('users', this.$strapi.user.id, payload)
        Swal.fire({
          text: 'User updated',
          icon: 'success'
        })

      },



    setUser(){
      const user = this.$strapi.user
      this.firstname = user.firstname
      this.lastname = user.lastname
      this.birthdate = user.birthdate
      this.street = user.street
      this.barangay = user.barangay
      this.city = user.city
      this.province = user.province
      this.email = user.email
      this.phone = user.phone
      this.username = user.username
    }
    },


    data () {
      return {
        isFormDisabled: true,
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
        confirm_password: ''

      }

    }
  }



</script>

