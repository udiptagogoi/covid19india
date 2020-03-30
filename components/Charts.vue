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
         <checkbox-svg-map :map="India"  @focus="focusLocation" v-model="selectedLocations" :location-class="getLocationClass" />
         <div class="text-xs font-medium mt-4" v-if="selected_region">
            <div class="px-4 py-3" role="alert">
               <p class="font-bold">{{selected_region.loc}}</p>
                <p class="text-gray-600 font-semibold mb-4 inline-block">TOTAL CASES : {{selected_region.total_cases}}</p>
                <p class="inline-block text-green-600 font-semibold mb-4">DISCHARGED : {{selected_region.discharged}}</p>
                <p class="inline-block text-red-600 font-semibold mb-4">DEATHS : {{selected_region.deaths}}</p>
            </div>
         </div>
         <div class="text-xs font-medium mt-4" v-else>
            <div class="px-4 py-3" role="alert" v-if="selected_region_name">
               <p class="font-bold">{{selected_region_name}}</p>
                <p class="text-gray-600 font-semibold mb-4 inline-block">NO CASES YET</p>
            </div>
         </div>
         <p class="text-xs font-bold text-indigo-700" v-if="!selected_region && !selected_region_name">
             CLICK ON THE REGION TO VIEW DETAILS
         </p>
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
            selected_region:'',
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
        focusLocation(event){
            this.selected_region = '';
            this.selected_region_name = '';
            var name = event.target.attributes.name.value;
            var loc = this.regions.find(region => region.loc === name);
            if(loc){
                this.selected_region = loc;
            }else{
                this.selected_region_name = name;
            }
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