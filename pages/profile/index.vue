<template>

  <b-container>

    <b-row>
      <strong> Email:  &nbsp; </strong> {{ user === null ? "" : user.email }}
    </b-row>
    <br>

    <b-row>

      <b-form-group label="Categorias">
        <b-form-checkbox-group id="cats" v-model="selected">
          <b-form-checkbox v-for="(category,key) in categories" :key="key" :value="category.id">
            {{category.name}}
          </b-form-checkbox>
        </b-form-checkbox-group>
      </b-form-group>

    </b-row>

    <br>
    <b-row>
      <b-button @click="save" variant="primary" style="margin-right:10px; margin-left:0"> Salvar </b-button>
    </b-row>

  </b-container>


</template>

<script>
export default{
  middleware : 'authenticated',
  data(){
    return {
      user : null,
      categories : [],
      selected: []
    };
  },

  head(){
    return {
      title: 'Perfil - InstaNews'
    }
  },

  mounted(){
    this.fetchUser();
  },
  methods:{

    fetchUser(){

      this.$axios.$get('users',{
        headers: {
          Authorization : this.$store.auth.accessToken
        }
      })
        .then(user => this.fetchCategories(user)[0])
        .then(categories => this.fetchUserCategories(categories))
        .then(userCategories => {
          userCategories.forEach(category => {
            this.selected.push(category.id)
          });
        })
        .catch(error => {
          this.failed(error)
        })
    },

    fetchCategories(user) {
      this.user = user
      return this.$axios.get('categories')
    },
    fetchUserCategories(categories) {

      this.categories = categories

      return this.$axios.$get('categories', {
        headers: {
          Authorization : this.$store.auth.accessToken
        }
      })

    },
    save(){

      let data = {
        categories : this.selected
      }

      this.$axios.$post('categories', data, {
        headers : {
          Authorization : this.$store.auth.accessToken
        }
      })
        .then(response => {
          this.$toast.success(response.message);
        })
        .catch(error => {
            this.failed(error)
        })
    },
    failed(error) {
      this.$toast.error(error)
    }
  }
}
</script>
