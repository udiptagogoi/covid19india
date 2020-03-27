<template>
  <div class="container bg-gray-100">
    <div class="flex flex-col">
      <div class="flex justify-center">
        <div class="mb-4 mt-4">
          <h6 class="text-sm font-semibold text-gray-700">
            Covid-19 Cases in India
          </h6>
        </div>
      </div>
      <div class="mb-4">
        <div class="flex flex-wrap mb-4 justify-center">
          <Summary :summary="summary" :is_regional="false" :last_update="last_update"/>
        </div>
        <div v-for="(region,index) in regional" :key="index" class="m-2">
          <Summary :summary="region" :is_regional="true"/>
        </div>
        
      </div>
    </div>
  </div>
</template>

<script>
import Logo from '~/components/Logo.vue'
import Summary from '~/components/Summary.vue'

export default {
  components: {
    Logo,
    Summary
  },
  data(){
    return{
      stats:[],
      summary:'',
      regional:'',
      last_update:'',
      last_origin_update:'',
    }
  },
  mounted(){
    this.loadStats();
  },
  methods:{
    async loadStats() {
      const stats = await this.$axios.$get('https://api.rootnet.in/covid19-in/stats/latest');
      this.stats = stats;
      this.summary = stats.data.summary;
      this.regional = stats.data.regional;
      this.last_update = stats.data.lastRefreshed;
      this.last_origin_update = stats.data.lastOriginUpdate;
    }
  }
}
</script>

<style>
/* Sample `apply` at-rules with Tailwind CSS
.container {
  @apply min-h-screen flex justify-center items-center text-center mx-auto;
}
*/
.container {
  margin: 0 auto;
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  text-align: center;
}

.title {
  font-family: 'Quicksand', 'Source Sans Pro', -apple-system, BlinkMacSystemFont,
    'Segoe UI', Roboto, 'Helvetica Neue', Arial, sans-serif;
  display: block;
  font-weight: 300;
  font-size: 100px;
  color: #35495e;
  letter-spacing: 1px;
}

.subtitle {
  font-weight: 300;
  font-size: 42px;
  color: #526488;
  word-spacing: 5px;
  padding-bottom: 15px;
}

.links {
  padding-top: 15px;
}
</style>
