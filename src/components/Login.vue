<template>
    <div class="container">
        <div class="row">
            <div class="col-md-6 mt-5 mx-auto">
                <form v-on:submit.prevent="login">
                    <h1 class="h3 mb-3 font-weight-normal">Please sign in</h1>
                    <div class="form-group">
                        <label for="email">Email Address</label>
                        <input type="email" v-model="email" class="form-control" name="email" placeholder="Enter Email">
                    </div>
                    <div class="form-group">
                        <label for="password">Password</label>
                        <input type="password" v-model="password" class="form-control" name="password" placeholder="Enter Password">
                    </div>
                    <div>
                      <p class="text-danger" >{{p}}</p>
                    </div>
                    <button class="btn btn-lg btn-primary btn-block">Sign in</button>
                </form>
            </div>
        </div>
    </div>

</template>

<script>
import axios from 'axios'
import router from '../router'
import EventBus from './EventBus'

export default {
  data () {
    return {
      email: '',
      password: '',
      p: ''
    }
  },

  methods: {
    login () {
      axios.post('users/login', {
        email: this.email,
        password: this.password
      }).then(res => {
        if (typeof res.data.token !== 'undefined') {
          localStorage.setItem('usertoken', res.data.token)
          this.email = ''
          this.password = ''
          this.p = ''
          router.push({ name: 'Profile' })
          this.emitMethod()
        } else {
          this.p = 'invalid email or password'
        }
      }).catch(err => {
        console.log(err)
      })
    },
    emitMethod () {
      EventBus.$emit('logged-in', 'loggedin')
    }
  }
}

</script>
