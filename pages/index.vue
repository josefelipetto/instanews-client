<template>
  <div>
    <div class="container">
      <BreakingNews />
    </div>

    <div class="container">
      <News />
    </div>
  </div>
</template>

<script>
import Logo from '~/components/Logo.vue'
import BreakingNews from '~/components/BreakingNews.vue'
import News from '~/components/News.vue'

const Cookie = process.client ? require('js-cookie') : undefined

export default {

  methods : {

    logout() {
      Cookie.remove('auth')
      this.$store.commit('setAuth', null)
    }
  },

  components: {
    Logo,
    BreakingNews,
    News
  },

  head(){
    return {
      title: 'Página principal - InstaNews'
    }
  }
}
</script>

<style>

.container {
  margin-top: 5vh
}


.links {
  padding-top: 15px;
}
</style>
