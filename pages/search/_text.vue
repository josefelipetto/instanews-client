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
                {{ article.description.substr(1,200).trim().slice(0,-3) + '...' }}
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
            let token = '62e7c1808232dea731a0c0d96669aba97bb5c779';

            let url = 'http://ec2-54-191-117-101.us-west-2.compute.amazonaws.com/';
            
            const news = await this.$axios.$get(url + "news/like/" + this.text);

            console.log(news)
            this.news = news;
       }
   }
}
</script>

<style>

</style>
