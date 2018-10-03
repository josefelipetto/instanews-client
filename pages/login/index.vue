<template>
    <b-container>
         <div>
             
           <Notification :message="error" v-if="error" />
            <b-form @submit="onSubmit" v-if="show" >
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
                
                <b-button type="submit" variant="primary">Login</b-button>
            </b-form>
        </div>
    </b-container>
</template>

<script>
import Notification from '~/components/Notification';

export default {
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
    onSubmit (evt) {
      evt.preventDefault();
      this.login();
    },
    async login(){
        try
        {
            await this.$auth.loginWith('local',{
                data: {
                    email: this.form.email
                }
            })
            .then(response => {
                console.log(response);
            })

            if(this.$auth.loggedIn)
            {
                this.$toast.success('Sucesso')
            }
            else
            {
                this.$toast.error('Erro')
            }

            this.$router.push('/')
        }
        catch(e)
        {
            this.error = 'Ocorreu algum erro ao processar sua requisição. '
        }
    }
  }
}
</script>

<!-- b-form-1.vue -->