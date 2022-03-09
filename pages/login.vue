<template>
<b-container fluid="md" class="mt-5 mb-5">
    <b-row>
      <b-col md="12">
        <b-card class="shadow-md border-0 rounded-lg">
           <h2 class="title is-3 has-text-grey">
      Login 
    </h2>
    <section>
        <b-field label="Email">
            <b-input name ='email' v-model="login.username" type="email"  maxlength="30" >
            </b-input>
        </b-field>

        <b-field label="Password">
            <b-input name= 'password' v-model="login.password" type="password" maxlength="30" ></b-input>
        </b-field>
       <b-button type="submit" variant="primary" @click="userLogin()">Log in</b-button>
        
    </section>
    </b-card>
     </b-col>
    </b-row>
  </b-container>
</template>
<script>
export default {
    name :"LoginPage",
  data() {
    return {
      login: {
        username: '',
        password: ''
      }
    }
  },
  methods: {
    async userLogin() {
      try {
        const response = await this.$auth.loginWith('http://localhost:8055/auth/login', { data: this.login })
        console.log(response)
       
        
    //     this.$auth.strategy.token.status().valid()
    //   window.location = 'http://localhost:3000/index'
        // })
      
      } catch (err) {
        console.log(err)
      }
    }
  }
}
</script>