<template>
    <div class="rounded overflow-hidden shadow-lg border-solid border-2 p-2" >
         <section class="flex justify-center p-4">
              <div class="m-2">
                  <div class="w-4 h-4 bg-red-600">

                  </div>
                  <div class="text-xs font-medium">
                    Affected
                  </div>
              </div>
              <div class="m-2">
                  <div class="w-4 h-4 bg-green-500">

                  </div>
                  <div class="text-xs font-medium">
                    Not Affected
                  </div>
              </div>
            </section>
         <checkbox-svg-map :map="India" is-location-selected="" v-model="selectedLocations" :location-class="getLocationClass" />
    </div>
</template>
<style scoped>

</style>
<script>
import { SvgMap } from "vue-svg-map";
import India from "@svg-maps/india";
import { CheckboxSvgMap } from "vue-svg-map";
export default {
    props:['regions'],
    components: {
     SvgMap,
     CheckboxSvgMap
    },
    data(){
        return{
            India,
            selectedLocations: [],
            regions_loaded:[],
        }
    },
    mounted(){
       
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
    },
    methods:{
        selected(){
            alert('ok');
        },
        getLocationClass(location, index){
            var loc = this.regions.find(region => region.loc === location.name);
            if(loc){
               return 'fill-current text-red-600';
            }else{
                return 'fill-current text-green-500';
            }
        }
    },
    
}
</script>