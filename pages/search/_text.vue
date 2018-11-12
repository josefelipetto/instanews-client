<template>
    <b-container>
        <b-row>
            Resultados para a pesquisa de : {{ text }}
        </b-row>
        <br>
        <b-row>

            <b-card v-for="(article,key) in news.articles" 
                    img-src=""
                    :key="key"
                    :title="article.Title"
                    img-alt="Image"
                    img-top
                    tag="article"
                    style="max-width: 20rem;margin-left: 40px; "
                    class="mb-2"
        >
            <p class="card-text">
                {{ article.description.substr(0,200).trim().slice(0,-3) + '...' }}
            </p>

            <b-button :href="article.url" variant="outline-success" style="margin-left:80px;">Ir a notícia</b-button>
            <div slot="footer">
                <small class="text-muted">{{ article.date }}</small>
            </div>
        </b-card>
            
        </b-row>
    </b-container>
</template>

<script>
export default {
    data(){
        return {
            text: '',
            news: []
        }
    },
    mounted(){
        this.getNews()
    },
    asyncData ({ params }) {
    // called every time before loading the component
    return {
      text: params.text
    }
  },
    
   methods:{
       async getNews(){
            this.news = await this.$axios.$get("news/v2/like/" + this.text);
       }
   }
}
</script>

<style>

</style>
