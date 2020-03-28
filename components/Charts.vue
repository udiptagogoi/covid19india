<template>
    <div class="rounded overflow-hidden shadow-lg border-solid border-2" >
        <D3BarChart :config="chart_config" :datum="regions_loaded" :title="chart_title" :source="chart_source"></D3BarChart>
    </div>
</template>
<script>
import { D3BarChart } from 'vue-d3-charts';
export default {
    props:['regions'],
    components: {
      D3BarChart
    },
    data(){
        return{
            chart_config :{
                key: 'loc',
                currentKey: '2007',
                values: ['total_cases','discharged','deaths'],
                axis: {
                  yTicks: 6
                },
                color: {
                    default: '#222f3e',
                    current: '#41B882'
                },
                transition: {ease: 'easeBounceOut', duration: 1000}
            },
            chart_title: 'Regional Statistics',
            chart_source: 'Your source goes here',
            regions_loaded:[],
            
        }
    },
    mounted(){
        this.regions_loaded = this.regions;
    },
    computed:{
       
        discharged_perc(){
            var f = Number(this.summary.discharged)/Number(this.summary.total);
            var p = Number(f)*100;
            return p.toFixed(2);
        },
        deaths_perc(){
            var f = Number(this.summary.deaths)/Number(this.summary.total);
            var p = Number(f)*100;
            return p.toFixed(2);
        }
    }
}
</script>