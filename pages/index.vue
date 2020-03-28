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
        <div class="chart w-screen sm:p-4 md:p-4 lg:p-4 overflow-x-auto"> 
          <div class="rounded overflow-x-auto shadow-lg border-solid border-2 " >
            <D3BarChart class="" :config="chart_config" v-if="regional.length>0" :datum="regional" :title="chart_title" ></D3BarChart>
            <section class="flex justify-center p-4">
              <div class="m-2">
                  <div class="w-4 h-4 bg-total">

                  </div>
                  <div class="text-xs font-medium">
                    Total Cases
                  </div>
              </div>
              <div class="m-2">
                  <div class="w-4 h-4 bg-discharged">

                  </div>
                  <div class="text-xs font-medium">
                    Discharged
                  </div>
              </div>
              <div class="m-2">
                  <div class="w-4 h-4 bg-death">

                  </div>
                  <div class="text-xs font-medium">
                    Deaths
                  </div>
              </div>
            </section>
            <section v-if="regional.length>0" class="m-2">
              <small class="inline-block text-gray-600" v-for="(reg,index) in regional" :key="index"><strong> <span> {{index}} </span> : <span>{{reg.loc}} ,</span></strong></small>
            </section>
            
          </div>
        </div>
        <div class="flex flex-wrap m-2 justify-center" v-for="(region,index) in regional" :key="index">
          <Summary :summary="region" :is_regional="true"/>
        </div>
        
      </div>
     
    </div>
    
  </div>
</template>

<script>
import Logo from '~/components/Logo.vue'
import Summary from '~/components/Summary.vue'
import Charts from '~/components/Charts.vue' 
import { D3BarChart } from 'vue-d3-charts';

export default {
  components: {
    Logo,
    Summary,
    Charts,
    D3BarChart,
  },
  data(){
    return{
      stats:[],
      summary:'',
      regional:[],
      last_update:'',
      last_origin_update:'',
      chart_config :{
          key: 'id',
          currentKey: '2007',
          values: ['total_cases','discharged','deaths'],
          axis: {
            yTicks: 12
          },
          color: {
            keys: {
              total_cases: '#222f3e',
              discharged: '#41B882',
              deaths: '#FF463D',
            },
          },
          transition: {ease: 'easeBounceOut', duration: 1000}
      },
      chart_title: 'Regional Statistics',
      // chart_source: 'Your source goes here',
    }
  },
  mounted(){
    this.loadStats();
  },
  computed:{
      statistics(){
            return this.regional;
        },
  },
  methods:{
    async loadStats() {
      const stats = await this.$axios.$get('https://api.rootnet.in/covid19-in/stats/latest');
      this.stats = stats;
      this.summary = stats.data.summary;
      this.regional = stats.data.regional;
      this.last_update = stats.data.lastRefreshed;
      this.last_origin_update = stats.data.lastOriginUpdate;
      this.mapRegions();
    },
    mapRegions(){
      var array = [];
      var array = this.regional.map(function(region,index){
          return {
            id: index,
            loc: region.loc,
            total_cases : Number(region.confirmedCasesIndian) + Number(region.confirmedCasesForeign),
            discharged : region.discharged,
            deaths : region.deaths
          }
      });
      this.regional = array;
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
.bg-total{
  background-color: #222f3e;
}
.bg-discharged{
  background-color: #41B882;
}
.bg-death{
  background-color: #FF463D;
}
</style>
