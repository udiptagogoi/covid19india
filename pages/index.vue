<template>
  <div class="container bg-gray-100">
    <div class="flex flex-col">
      <div class="flex justify-center">
        <div class="mb-4 mt-4">
          <h6 class="text-sm font-semibold text-gray-700">
            COVID-19 Cases in India
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
        <!-- <div class="flex flex-wrap m-2 justify-center" v-for="(region,index) in regional2" :key="index">
          <Summary :summary="region" :is_regional="true"/>
        </div> -->
        <div class="map mt-2">
          <Charts :regions="regional" v-if="regional" />
        </div>
        <div class="chart w-screen sm:p-4 md:p-4 lg:p-4 ">
          <div class="rounded overflow-hidden shadow-lg border-solid border-2 p-4 md:p-8 lg:p-8">
              
              <div class="bg-teal-100 border-t-4 border-teal-500 rounded-b text-teal-900 px-2 py-3 shadow-md" role="alert">
                <div class="flex">
                  <div>
                  <div class="py-1"><svg class="fill-current h-6 w-6 text-teal-500 mr-4" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 20 20"><path d="M2.93 17.07A10 10 0 1 1 17.07 2.93 10 10 0 0 1 2.93 17.07zm12.73-1.41A8 8 0 1 0 4.34 4.34a8 8 0 0 0 11.32 11.32zM9 11V9h2v6H9v-4zm0-6h2v2H9V5z"/></svg></div>
                    <p class="text-sm text-justify">The Government of Assam has converted all Government Hospitals into COVID-19 treatment centers considering the current pandemic situation. The Government has also arranged for the empanelled hospitals listed below to offer cashless treatment tp all AAA and PMJAY beneficiaries for varuous ailments . 
                    </p>
                  </div>
                </div>
              </div>
              
              <div class="p-2 text-sm font-medium mb-2 bg-blue-500 text-white">
                The following specialities are available in the empanelled private hospitals mentioned below .
              </div>
              <p class="text-left text-sm font-medium mb-2" v-for="(hospital,index) in hospitals" :key="index">
                <span class="text-gray-800">{{index+1}}.</span> <span class="text-gray-700">{{hospital.name}}</span> <br>
                <section class="border-solid border-2 p-2">
                  <span class=""><span class="text-gray-800">Address :</span>  <span class="text-gray-600">{{hospital.address}}</span> </span> <br>
                  <span class=""><span class="text-gray-800">Speciality :</span> <span class="text-gray-600">{{hospital.speciality}}</span> </span>
                </section>
              </p>
          </div>
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
      hospitals:[
        {id:'1',name:'GNRC, Dispur',address:'Dispur,Guwahati',speciality:'Paediatric, Dermatology, Neurology, Neurosurgery, Cardiology, Urology, Nephrology, General Medicine, Orthopaedic , All eye diseases, ENT, Maternity, General Surgery, Neuropsychiatry, CTVS, Diabetology, O&G, ICU, Blood Bank Service, Critical Care & Trauma, Accident & Emergency, NICU, Gastroentirology, Neurophysiology'},
        {id:'2',name:'GNRC, Six Mile',address:'Six Mile,Guwahati',speciality:'Paediatric, Dermatology, Neurology, Neurosurgery, Cardiology, Urology, Nephrology, General Medicine, Orthopaedic , All eye diseases, ENT, Maternity, General Surgery, Neuropsychiatry, CTVS, Diabetology, O&G, ICU, Blood Bank Service, Critical Care & Trauma, Accident & Emergency, NICU, Gastroentirology, Neurophysiology'},
        {id:'3',name:'GNRC, North Guwahati',address:'Amingaon,Guwahati',speciality:'Paediatric, Dermatology, Neurology, Neurosurgery, Cardiology, Urology, Nephrology, General Medicine, Orthopaedic , All eye diseases, ENT, Maternity, General Surgery, Neuropsychiatry, CTVS, Diabetology, O&G, ICU, Blood Bank Service, Critical Care & Trauma, Accident & Emergency, NICU, Gastroentirology, Neurophysiology'},
        {id:'4',name:'Marwari Hospital',address:'Athgaon Guwahati-8',speciality:'Paediatric, General Medicine, Maternity, Neonatology, General Surgery, Dialysis, Orthopaedic, ICU, Blood Bank Service, X-Ray, USG, CTScan, O & G includingneonatal care (NICU)'},
        {id:'5',name:'Nemcare Hospital',address:'G.S. Road Bhangaghar, Guwahati',speciality:'Cardiology, Neurology, Urology, Orthopaedic, Nephrology, General Surgery, General Medicine, Maternity, ENT, ICU, Burn'},
        {id:'6',name:'Agile Hospital Pvt. Ltd.',address:'Jayanagar Chariali, Tripura Road, Guwahati-22',speciality:'Neurosurgery, Neurology, Orthopaedic, Lap Surgery, General Surgery, General Medicine, Maternity, ENT, ICU'},
        {id:'7',name:'Sri Sankardev Netralaya',address:'Beltola, Guwahati-20',speciality:'All eye diseases'},
        {id:'8',name:'Sanjivani Hospital',address:'Maligaon, Guwahati-20',speciality:'Medicine, Paediatric, General Surgery, Maternity, ENT, ICU'},
        {id:'9',name:'Eye Doctors Hospital',address:'Mayur Heights, ABC, G.S Road, Guwahati-20',speciality:'All eye diseases'},
        {id:'10',name:'Sun Valley Hospital',address:'G.S Road, Near down town Hospital, Guwahati-20',speciality:'Diabetes Mellitus'},
        {id:'11',name:'Rahman Hospital',address:'Six Mile, Guwahati-22',speciality:'Neurosurgery, General Surgery'},
        {id:'12',name:'Hayat Hospital',address:'Lal ganesh, Udalbhakra, Guwahati-34',speciality:'Cardiology, CTVs, Neurology, Neurosurgery, Nephrology, Urology, Medicine, General Surgery, Critical Care, Trauma'},
        {id:'13',name:'Ayusundra Superspeciality Hospital',address:'Gorchuk, Ahom Gaon, Betkuchi',speciality:'Orthopaedic , Surgery, Trauma, Cardiology, Neurosurgery, General Surgery, ENT, Gastroentirology, General Medicine, Emergency, Critical Care, ICU, Paediatric'},
        {id:'14',name:'KGMT',address:'Gitanagar, Guwahati',speciality:'Paediatric, Paediatric Surgery, Maternity, General Medicine, Pain & Paliative, NICU, Geriatric Medicine, Orthopaedic, ICU, General Surgery'},
        {id:'15',name:'Dispur Hospital',address:'Ganeshguri, Guwahati',speciality:'General Surgery, General Medicine, Urology, Nephrology, Cardiology, ICU Package , AES/JE '},
        {id:'16',name:'Apollo  Hospital',address:'G.S Road, Chritian Basti, Guwahati-5',speciality:'Cardiology, CTVs, Neurosurgery, Orthopaedics, Urology, Gastroentirology.'},
        {id:'17',name:'Wintrobe  Hospital',address:'GNB Road, Ambari, Guwahati-1',speciality:'Medicine, General Surgery, Urology, Maternity, Orthopaedic, Paediatric, Neurology, Cardiology, ENT, Psychiatry, Nephrology, ICU'},
        {id:'18',name:'Aruna Memomrial Hospital',address:'Rajgarh Road, Bhangaghar, Guwahati-16',speciality:'General Surgery, Maternity, Orthopaedic'},
        {id:'19',name:'Lion Eye Hospital',address:'Near KC Das Commerce College',speciality:'All eye diseases'},
        {id:'20',name:'Central Nursing Home',address:'Beltola, Guwahati',speciality:'General Surgery, Maternity, Paediatric, Urology'},
        {id:'21',name:'Dispur Polyclinic',address:'Ganeshguri, Guwahati-6',speciality:'General Medicine, General Surgery, Orthopaedic, Maternity, ENT, ICU, Dialysis, Neurology, Neurosurgery, Urology, Nephrology'},
        {id:'22',name:'Narayan Hrudalaya',address:'Amingaon, Guwahati',speciality:'Cardiology Surgery, Urology, Nephrology, Gastroentirology, General Surgery, Neurology'},
        {id:'23',name:'Gate Hospital',address:'Narengi, Guwahati',speciality:'Neurosurgery, Urology, ICU, General Surgery, Orthopaedic, Maternity, Dialysis, AES/JE'},
        {id:'24',name:'Nightingale Hospital',address:'Kacharibasti, Guwahati-5',speciality:'ENT'},
        {id:'25',name:'Healthcity Hospital',address:'Khanapara, Guwahati',speciality:'Radiation Oncology, Medical Oncology, Surgical Oncology, Critical Cancer Patients, All emergency Cardiac Patient (Medical & Surgical) , Dialysis'},
        {id:'26',name:'Satribari Christian Hospital',address:'Satribari, Guwahati-9',speciality:'Medicine, General Surgery, Maternity, Paediatric, NICU'},
        {id:'27',name:'Dr. B A Saikia Memorial Nursing Home',address:'Adabari, Guwahati',speciality:'General Surgery, Maternity'},
        {id:'28',name:'Barthakur Clinic',address:'Kharghuli, Guwahati',speciality:'Medicine, General Surgery, Maternity, ENT, Paediatric'},
        {id:'29',name:'Swagat Super Speciality Hospital',address:'Maligaon, Guwahati',speciality:'General Surgery, Lap. Surgery, GI & Hepatobiliary Surgery, Urology, Neurosurgery, Orthopaedic, ICU'},
        {id:'30',name:'East End Nursing Home',address:'Bamunimaidam, Guwahati',speciality:'General Surgery, Medicine, Maternity, All eye diseases, ENT'},
        {id:'31',name:'Kalicharan Das Nursing Home',address:'Kalapahar, Guwahati',speciality:' Maternity, Paediatric'},
        {id:'32',name:'Down Town Hospital',address:'Down Town, Guwahati',speciality:' General Surgery, Medicine, Urology, Nephrology, Cardiology, ENT, Orthopaedic, Burn, Maternity, Dermatology'},
        {id:'33',name:'Critical Care Hospital, Guwahati',address:'Lakhra, Guwahati',speciality:'General Medicine, Psychiatry, General Surgery, Urology, ICU, Neurosurgery'},
        {id:'34',name:'Excelcare Hospitals',address:'Boragaon, Guwahati',speciality:'Cardiac Surgery, Neurosurgery, Urology, Emergency & Trauma, General Medicine, Paediatric, Cardiology'},
        {id:'35',name:'Pratiksha Hospital',address:'VIP Road, Guwahati',speciality:'Maternity, Paediatric'},
        {id:'36',name:'Arya Hospital',address:'Rihabari, Guwahati',speciality:'General Surgery, General Medicine, Orthopaedics, Maternity, Neurosurgery, Urology, Dialysis, Paediatrics'},

      ],
      stats:[],
      summary:'',
      regional:[],
      regional2:[],
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
      this.regional2 = stats.data.regional;
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
