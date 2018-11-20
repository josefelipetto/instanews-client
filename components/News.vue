<template>
    <div>
        <b-tabs >
            <b-tab v-for="tab in tabs" :title="tab.category.name" :key="tab.category.id" >
                <br>
                <b-row>

                    <b-card v-for="(article,key) in tab.articles"
                            img-src=""
                            :key="key"
                            :title="article._source.title"
                            img-alt="Image"
                            img-top
                            tag="article"
                            style="max-width: 20rem;margin-left: 40px; "
                            class="mb-2"
                    >
                        <p class="card-text" v-if="article._source.description != null">
                            {{ article._source.description.substr(0,200).trim().slice(0,-3) + '...' }}
                        </p>

                        <b-button :href="article._source.newsurl" variant="outline-success" style="margin-left:80px;">Ir a notícia</b-button>
                        <div slot="footer">
                            <small class="text-muted">{{ article._source.date }}</small>
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

            const categories = await this.$axios.$get('categories',{
                headers: {
                    Authorization : token
                }
            })
            console.log(categories);
            categories.forEach(category => {
                this.$axios
                    .$get("news/v2/category/" + category.id,{

                        headers : {
                            Authorization : token
                        }
                    })
                    .then(response => {

                        response.articles.forEach(el => {
                            el._source.date = moment(el._source.date).format('MMMM Do YYYY, h:mm:ss a');
                            el._source.description = el._source.description !== "undefined" ? el._source.description : null;
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

