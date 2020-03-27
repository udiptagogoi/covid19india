<template>
    <div class="max-w-sm rounded overflow-hidden shadow-lg border-solid border-2" :class="!is_regional ? 'bg-indigo-100' : ''">
        <div class="px-6 py-4 min-h-64 text-sm" v-if="!is_regional">
            <p class="text-center text-gray-600 font-semibold mb-4">Total Cases : {{summary.total}}</p>
            <p class="text-left text-gray-600 font-semibold mb-4 inline-block">Confirm(Indian) : {{summary.confirmedCasesIndian}}</p>
            <p class="text-left text-gray-600 font-semibold mb-4 inline-block">Confirm(Foreign) : {{summary.confirmedCasesForeign}}</p>
            <p class="text-left inline-block text-green-600 font-semibold mb-4">Discharged : {{summary.discharged}} ({{discharged_perc}}%)</p>
            <p class="text-left inline-block text-red-600 font-semibold mb-4">Deaths : {{summary.deaths}} ({{deaths_perc}}%)</p>
        </div>
        <div class="px-6 py-4 min-h-64 text-sm" v-else>
            <p class="text-center text-gray-800 font-semibold mb-4">Location : {{summary.loc}}</p>
            <p class="text-left text-gray-600 font-semibold mb-4 inline-block">Confirm(Indian) : {{summary.confirmedCasesIndian}}</p>
            <p class="text-left text-gray-600 font-semibold mb-4 inline-block">Confirm(Foreign) : {{summary.confirmedCasesForeign}}</p>
            <p class="text-left inline-block text-green-600 font-semibold mb-4">Discharged : {{summary.discharged}}</p>
            <p class="text-left inline-block text-red-600 font-semibold mb-4">Deaths : {{summary.deaths}}</p>
        </div>
    </div>
</template>
<script>
export default {
    props:['summary','is_regional','last_update'],
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