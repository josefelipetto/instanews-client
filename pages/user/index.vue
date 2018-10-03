<template>
  <div>
    <Navigation />

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
import Navigation from '~/components/Navigation.vue'
import BreakingNews from '~/components/BreakingNews.vue'
import News from '~/components/News.vue'

export default {
  components: {
    Logo,
    Navigation,
    BreakingNews,
    News
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
