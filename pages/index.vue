<template>
  <div class="grid bg-gray-100 h-screen w-screen place-content-center">
    <div class="bg-white max-w-2xl w-78 rounded-md shadow-md p-8">
      <img
        src="https://kompletecare.com/logo.png"
        alt=""
        class="w-full h-12 object-contain"
      />

      <h1 class="text-gray-600 text-center text-2xl mt-4 font-medium">
        Sign in to start your session
      </h1>

      <div class="space-y-4 mt-8">
        <div class="space-y-3">
          <label class="text-gray-600 text-base font-medium">Email</label>
          <input
            type="text"
            class="focus:ring-red-50 outline-none placeholder:text-black border-gray-100 bg-blue-100 shadow border rounded-md p-4 flex flex-grow w-full"
            placeholder="Email Address"
            v-model.trim="email"
          />
        </div>
        <div class="space-y-3">
          <label class="text-gray-600 text-base font-medium">Password</label>
          <input
            type="password"
            class="focus:ring-red-50 outline-none placeholder:text-black border-gray-100 bg-blue-100 shadow border rounded-md p-4 w-full"
            placeholder="***********"
            v-model.trim="password"
          />
        </div>
      </div>
      <button
        @click="userLogin"
        class="px-5 flex justify-center mt-5 outline-none w-full py-3 bg-blue-800 text-white text-lg rounded-md hover:bg-blue-900 transform-translate duration-500"
      >
        <span v-if="!loading">Login</span>
        <div v-else class="flex justify-center items-center space-x-4">
          <div
            class="h-6 w-6 rounded-full border-4 border-t-green-600 border-r-green-600 border-b-green-100 border-l-green-100 animate-spin"
          ></div>
          <span> login in... </span>
        </div>
      </button>
      <h3 class="text-center text-red-500 font-medium my-4" v-if="error">
        {{ error }}
      </h3>
    </div>
  </div>
</template>

<script>
import gql from 'graphql-tag'

export default {
  name: 'IndexPage',
  head() {
    return {
      title: 'Login | Best telemedicine platform in Africa',
    }
  },
  data() {
    return {
      email: 'tester@kompletecare.com',
      password: 'password',
      error: null,
      loading: false,
    }
  },
  methods: {
    async userLogin() {
      this.error = ''
      this.loading = true
      this.$apollo
        .mutate({
          mutation: gql`
            mutation login($email: String!, $password: String!) {
              login(email: $email, password: $password)
            }
          `,
          variables: {
            email: this.email,
            password: this.password,
          },
        })
        .then((res) => {
          const token = res.data.login
          this.$apolloHelpers.onLogin(token)
          this.loading = false

          this.$router.push('/dashboard')
        })
        .catch((err) => {
          this.error = err.message
          this.loading = false
        })
    },
  },
}
</script>
