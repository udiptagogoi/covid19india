
<template>
    <div id="stage" style="width: 120px; height: 120px;">
        <div class="spinner">
            <div class="face1 ">
               <strong> COVID-19 <br>
                INDIA</strong>
            </div>
            <div class="face2 text-green">
                {{summary.discharged}} <br/>
                <small><strong>DISCHARGED</strong></small>
            </div>
            <div class="face3 text-blue">
                {{summary.total}} <br/>
                <small><strong>CASES</strong></small>
            </div>
            <div class="face4 text-indigo">
                {{summary.confirmedCasesIndian}} <br/>
                <small><strong>INDIAN</strong></small>
            </div>
            <div class="face5 text-purple">
                 {{summary.confirmedCasesForeign}} <br/>
                <small><strong>FOREIGN</strong></small>
            </div>
            <div class="face6 text-red">
                {{summary.deaths}}<br/>
                <small><strong>DEATHS</strong></small>
            </div>
        </div>
    </div>
</template>
<script>
export default {
    props:['summary'],
    data(){
        return{

        }
    }
}
</script>
<style>
.spinner div {
    position: absolute;
    width: 120px;
    height: 120px;
    border: 1px solid #ccc;
    background: rgba(255,255,255,0.8);
    box-shadow: inset 0 0 20px rgba(0,0,0,0.2);
    text-align: center;
    /* line-height: 120px; */
    font-size: 18px;
}

.spinner .face1 { 
    -webkit-transform: translateZ(60px);
    -ms-transform: translateZ(60px);
    transform: translateZ(60px);
}
.spinner .face2 { 
    -webkit-transform: rotateY(90deg) translateZ(60px); 
    -ms-transform: rotateY(90deg) translateZ(60px); 
    transform: rotateY(90deg) translateZ(60px); 
}
.spinner .face3 { 
    -webkit-transform: rotateY(90deg) rotateX(90deg) translateZ(60px); 
    -ms-transform: rotateY(90deg) rotateX(90deg) translateZ(60px); 
    transform: rotateY(90deg) rotateX(90deg) translateZ(60px); 
}
.spinner .face4 { 
    -webkit-transform: rotateY(180deg) rotateZ(90deg) translateZ(60px); 
    -ms-transform: rotateY(180deg) rotateZ(90deg) translateZ(60px); 
    transform: rotateY(180deg) rotateZ(90deg) translateZ(60px); 
}
.spinner .face5 { 
    -webkit-transform: rotateY(-90deg) rotateZ(90deg) translateZ(60px); 
    -ms-transform: rotateY(-90deg) rotateZ(90deg) translateZ(60px); 
    transform: rotateY(-90deg) rotateZ(90deg) translateZ(60px); 
}
.spinner .face6 { 
    -webkit-transform: rotateX(-90deg) translateZ(60px); 
    -ms-transform: rotateX(-90deg) translateZ(60px); 
    transform: rotateX(-90deg) translateZ(60px); 
}

.spinner {
    -webkit-animation: spincube 24s ease-in-out infinite;
    animation: spincube 24s ease-in-out infinite;
    -webkit-transform-style: preserve-3d;
    -ms-transform-style: preserve-3d;
    transform-style: preserve-3d;
    -webkit-transform-origin: 60px 60px 0;
    -ms-transform-origin: 60px 60px 0;
    transform-origin: 60px 60px 0;
}

@-webkit-keyframes spincube {
    16%      { -webkit-transform: rotateY(-90deg);                }
    33%      { -webkit-transform: rotateY(-90deg) rotateZ(90deg); }
    50%      { -webkit-transform: rotateY(180deg) rotateZ(90deg); }
    66%      { -webkit-transform: rotateY(90deg) rotateX(90deg);  }
    83%      { -webkit-transform: rotateX(90deg);                 }
}
@keyframes spincube {
    16%      { -ms-transform: rotateY(-90deg); transform: rotateY(-90deg); }
    33%      { -ms-transform: rotateY(-90deg) rotateZ(90deg); transform: rotateY(-90deg) rotateZ(90deg); }
    50%      { -ms-transform: rotateY(180deg) rotateZ(90deg); transform: rotateY(180deg) rotateZ(90deg); }
    66%      { -ms-transform: rotateY(90deg) rotateX(90deg); transform: rotateY(90deg) rotateX(90deg); }
    83%      { -ms-transform: rotateX(90deg); transform: rotateX(90deg); }
}
.text-red{
    color: red;
}
.text-green{
    color:green;
}
.text-blue{
    color: #2b6cb0;
}
.text-indigo{
    color: #4c51bf;
}
.text-purple{
    color: #6b46c1;
}
</style>
