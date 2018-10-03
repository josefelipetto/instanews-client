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

    async fetchUser(){
      
      let token = '62e7c1808232dea731a0c0d96669aba97bb5c779';

      let url = 'http://localhost:3001/';

      const user = await this.$axios.$get(url + 'users',{
        headers: {
          Authorization : token
        }
      });

      this.user = user[0];

      let categories = await this.$axios.$get(url + 'categories');

      let userCategories = await this.$axios.$get(url + "categories", {
        
        headers: {
          Authorization : token
        }
      
      });

      let selected = [];

      userCategories.forEach(element => {
        selected.push(element.id)
      });

      this.categories = categories;

      this.selected = selected;
    },

    async save(){
      console.log('aaa')
      let token = '62e7c1808232dea731a0c0d96669aba97bb5c779';

      let url = 'http://localhost:3001/';
      
      let response = await this.$axios.$post(url + "categories", {
        categories : this.selected
      }, {
        headers : {
          Authorization : token
        }
      });

      this.$toast.success(response.message);
    }
  }
}
</script>