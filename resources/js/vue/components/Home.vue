<template>
  <div class="flex min-h-full items-center justify-center py-12 px-4 sm:px-6 lg:px-8 mt-24">
    <div class="w-full max-w-md space-y-8">
      <div>
        <h2 class="mt-6 text-center text-3xl font-bold tracking-tight text-gray-900">Sign in to your account</h2>
      </div>
      <form class="mt-8 space-y-6" v-on:submit.prevent="login()">
        <input type="hidden" name="remember" value="true" />
        <div class="-space-y-px rounded-md shadow-sm">
          <div>
            <label for="email-address" class="sr-only">Email address</label>
            <input v-model="email" autocomplete="email" class="relative block w-full appearance-none rounded-none rounded-t-md border border-gray-300 px-3 py-2 text-gray-900 placeholder-gray-500 focus:z-10 focus:border-indigo-500 focus:outline-none focus:ring-indigo-500 sm:text-sm" placeholder="Email address" />
            <span class="text-red-500" v-if="errors.length>0">{{errors.email[0]}}</span>
          </div>
          <div>
            <label for="password" class="sr-only">Password</label>
            <input v-model="password" type="password" autocomplete="current-password" class="relative block w-full appearance-none rounded-none rounded-b-md border border-gray-300 px-3 py-2 text-gray-900 placeholder-gray-500 focus:z-10 focus:border-indigo-500 focus:outline-none focus:ring-indigo-500 sm:text-sm my-8" placeholder="Password" />
            <span class="text-red-500" v-if="errors.length>0">{{errors.password[0]}}</span>
          </div>
        </div>

        <div class="flex items-center justify-between">
          <div class="flex items-center">
            <input id="remember-me" name="remember-me" type="checkbox" class="h-4 w-4 rounded border-gray-300 text-indigo-600 focus:ring-indigo-500" />
            <label for="remember-me" class="ml-2 block text-sm text-gray-900">Remember me</label>
          </div>

          <div class="text-sm">
            <router-link to="/register"  class="font-medium text-indigo-600 hover:text-indigo-500">Register</router-link>
         </div>
        </div>

        <div>
          <button type="submit" class="group relative flex w-full justify-center rounded-md border border-transparent bg-indigo-600 py-2 px-4 text-sm font-medium text-white hover:bg-indigo-700 focus:outline-none focus:ring-2 focus:ring-indigo-500 focus:ring-offset-2">
            <span class="absolute inset-y-0 left-0 flex items-center pl-3">
              <LockClosedIcon class="h-5 w-5 text-indigo-500 group-hover:text-indigo-400" aria-hidden="true" />
            </span>
            Sign in
          </button>
        </div>
      </form>
      <div class="text-sm">
            </div>
    </div>
  </div>
</template>
<script>
import axios from 'axios'
export default{
    name:'Home',
    data(){
        return {
            email:'',
            password:'',
            errors:[]
        }
    },
    methods:{
      async login()
      {
        try{
          let response= await axios.post('api/loginuser',{
          email:this.email,password:this.password
         }); 
         if(response.status==200)
         {
          localStorage.setItem('authtoken', response.data)
          this.$router.push('/todos')
         }
        }catch(error)
        {
            console.log(error.response.data.errors)
            this.errors=error.response.data.errors
        }
      }
    },  
  mounted(){
    if(localStorage.getItem('authtoken'))
      {
        this.$router.push('/todos')
      }
    }
}
</script>