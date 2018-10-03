<template>
    <div>
        <b-tabs >
            <b-tab v-for="tab in tabs" :title="tab.category.name" :key="tab.category.id" >
                <br>
                <b-row>
                    
                    <b-card v-for="(article,key) in tab.articles" 
                            img-src="https://picsum.photos/600/300/?image=25"
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
            </b-tab>
        </b-tabs>
    </div>
</template>

<script>

import moment from 'moment';

moment.locale('pt-BR');


export default{
    data(){
        return {
            tabs: null
        }
    },
    mounted(){
        this.fetchNews()
    },
    methods: {
        async fetchNews() {
            
            let token = '62e7c1808232dea731a0c0d96669aba97bb5c779';
           
            let news = [];

            let url = 'http://localhost:3001/';

            const categories = await this.$axios.$get(url + 'categories',{
                headers: {
                    Authorization : token
                }
            })
            
            categories.forEach(category => {
                this.$axios
                    .$get(url + "news/category/" + category.id,{
                        
                        headers : {
                            Authorization : token
                        }
                    })
                    .then(response => {

                        response.articles.forEach(el => {
                            el.date = moment(el.date).fromNow()
                        })

                        news.push({

                            category:{
                                id: category.id,
                                name: category.name
                            },

                            articles: response.articles
                        });        
                    });
            });

            this.tabs = news

        }
    }

}
</script>

