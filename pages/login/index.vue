<template>
    <b-container>
         <div>
             
           <Notification :message="error" v-if="error" />
            <b-form v-if="show" >
                <b-form-group id="email"
                                label="Endereço de e-mail:"
                                label-for="email"
                >
                    <b-form-input id="email"
                                type="email"
                                v-model="form.email"
                                required
                                placeholder="johndoe@example.com">
                    </b-form-input>
                </b-form-group>
                
                <b-button type="button" variant="primary" @click="postLogin">Login</b-button>
            </b-form>
        </div>
    </b-container>
</template>

<script>
import Notification from '~/components/Notification';
const Cookie = process.client ? require('js-cookie') : undefined;

export default {

  middleware : 'notAuthenticated',

  components: {
      Notification,
  },

  data () {
    return {
      
      form: {
        email: '',
      },

      showError : false,

      error : null,
      
      show: true
    }
  },
  head(){
    return {
      title: 'Login - InstaNews'
    }
  },
  methods: {
    postLogin() {
      this.$axios.post('login', {
        'email' : this.form.email
      })
        .then((response) => {

          const auth = {
            accessToken : response.data.token
          }

          this.$store.commit('setAuth',auth);

          Cookie.set('auth',auth);

          this.$router.push('/');

        })
    }
  }
}
</script>

<!-- b-form-1.vue -->
