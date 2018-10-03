<template>
    <b-container>
         <div>
             
           <Notification :message="error" v-if="error" />
            <b-form @submit="onSubmit" v-if="show">
            
            <b-form-group id="email"
                            label="Endereço de e-mail:"
                            label-for="email"
                            description="Nunca vamos compartilhar esta informação com ninguém.">
                <b-form-input id="email"
                            type="email"
                            v-model="form.email"
                            required
                            placeholder="johndoe@example.com">
                </b-form-input>
            </b-form-group>
            
            <b-button type="submit" variant="primary">Registrar</b-button>
            
            <p style="margin-top: 20px">
                Already got an account? <nuxt-link to="/login">Login</nuxt-link>
            </p>

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
      title: 'Registre-se - InstaNews'
    }
  },
  methods: {
    onSubmit (evt) {
      evt.preventDefault();
      this.register();
    },
    async register(){
        try
        {
            await this.$axios.$post('users',{
                email: this.form.email
            })
            .then(response => {
                console.log(response);
            })

            this.$toast.success('Usuário criado com sucesso');

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