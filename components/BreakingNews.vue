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

      <b-carousel-slide v-for="article in news" 
                        :key="article._source.idnews"
                        :caption="article._source.title"
                        :img-src="article._source.thumbnail ? article._source.thumbnail:''"
                        :href="article._source.url"
        

      > 
      
      </b-carousel-slide>

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

      let url = 'http://ec2-54-191-117-101.us-west-2.compute.amazonaws.com/';

      const breakingNews = await this.$axios.$get(url + "news/v2/breaking");
     
      this.news =  breakingNews.articles; 
    }
  }
}
</script>