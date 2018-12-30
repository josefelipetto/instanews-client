<template>
  <div >
    <b-carousel id="carousel1"
                style="text-shadow: 1px 1px 2px #333; height: 480px;"
                controls
                indicators
                background="#ababab"
                :interval="4000"
                img-width="1024"
                img-height="480"
                v-model="slide"
                @sliding-start="onSlideStart"
                @sliding-end="onSlideEnd"
    >
      <b-link v-for="(article,key) in news" :href="article._source.url" :key="key">
        <b-carousel-slide :key="article._source.idnews"
                          :img-src="article._source.thumbnail"
                          style="max-width:1110px;max-height:488px;width: auto;height: auto;"

        >
            {{ article._source.title }}

        </b-carousel-slide>
      </b-link>

    </b-carousel>

  </div>
</template>

<script>

export default {

  data () {
    return {
      slide: 0,
      sliding: null,
      news: null
    }
  },
  mounted(){
    this.fetchNews()
  },
  methods: {
    onSlideStart (slide) {
      this.sliding = true
    },
    onSlideEnd (slide) {
      this.sliding = false
    },
    async fetchNews(){

      let breakingNewsBanner = 'https://socialmediaweek.org/wp-content/blogs.dir/1/files/breaking-news-feature.jpg'

      const breakingNews = await this.$axios.$get("news/v2/breaking");

      this.news =  breakingNews.articles;

      this.news.forEach(element => {

        element._source.thumbnail = element._source.thumbnail ?
          (element._source.thumbnail === 'https://picsum.photos/600/300/?image=25' ? breakingNewsBanner : element._source.thumbnail)
          : breakingNewsBanner

        if(element._source.thumbnail[element._source.thumbnail.length -1] === '"') {
          element._source.thumbnail = element._source.thumbnail.substr(0,element._source.thumbnail.length -1 );
        }

      })

    }
  }
}
</script>
