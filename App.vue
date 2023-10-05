<script setup>
import { ref } from "vue"

const thisvideo=ref();
const videoget=ref();
const videofrom=ref();
const endtime=ref("00:00")
function playbutton(event) {
    videoget.value=event.target.files[0];
    videofrom.value=URL.createObjectURL(videoget.value);
    thisvideo.value.oncanplay=function(){undefined
        //get to know the total time of the video
        const duration = thisvideo.value.duration;
        const hour1 = parseInt(duration / 3600).toString().padStart(2,"0");
        const minute1 =parseInt(duration % 3600 / 60).toString().padStart(2,"0");
        const sec1 = parseInt(duration % 60).toString().padStart(2,"0");
        endtime.value=hour1+':'+minute1+':'+sec1;
    }
};

const icon1=ref();
const icon2=ref();
const starttime=ref("00:00")
const barwidth=ref()
function playstop(){
    //let video play and change the icon
    if(thisvideo.value.paused){
        thisvideo.value.play();
        icon1.value.style.display="none";
        icon2.value.style.display="block";
    }else{
        thisvideo.value.pause();
        icon1.value.style.display="block";
        icon2.value.style.display="none";
    }

    thisvideo.value.ontimeupdate=function(){
        //get to know the current playing time of the video
        const duration = thisvideo.value.duration;
        const currenttime=thisvideo.value.currentTime;
        const hour = parseInt(currenttime / 3600).toString().padStart(2,"0");
        const minute =parseInt(currenttime % 3600 / 60).toString().padStart(2,"0");
        const sec = parseInt(currenttime % 60).toString().padStart(2,"0");
        starttime.value=hour+':'+minute+':'+sec
        

        //display the progress of current time ofthe video
        const progress = (currenttime / duration) * 98.6;
        barwidth.value.style.width=progress+'%';
        
        //play over and change back the icon
        if(currenttime==duration){
            icon1.value.style.display="block";
            icon2.value.style.display="none";
        }
    }
}

function godown1(){
    const duration = thisvideo.value.duration;
    let currenttime=thisvideo.value.currentTime+10;
    if(currenttime>duration){currenttime=duration};
    const hour = parseInt(currenttime / 3600).toString().padStart(2,"0");
    const minute =parseInt(currenttime % 3600 / 60).toString().padStart(2,"0");
    const sec = parseInt(currenttime % 60).toString().padStart(2,"0");
    starttime.value=hour+':'+minute+':'+sec
    const progress = (currenttime/duration) * 98.6;
    barwidth.value.style.width=progress+'%';
    thisvideo.value.currentTime=currenttime
    thisvideo.value.play();
    icon1.value.style.display="none";
    icon2.value.style.display="block";
}

function godown2(){
    const duration = thisvideo.value.duration;
    let currenttime=thisvideo.value.currentTime-10;
    if(currenttime<0){currenttime=0};
    const hour = parseInt(currenttime / 3600).toString().padStart(2,"0");
    const minute =parseInt(currenttime % 3600 / 60).toString().padStart(2,"0");
    const sec = parseInt(currenttime % 60).toString().padStart(2,"0");
    starttime.value=hour+':'+minute+':'+sec
    const progress = (currenttime/duration) * 98.6;
    barwidth.value.style.width=progress+'%';
    thisvideo.value.currentTime=currenttime
    thisvideo.value.play();
    icon1.value.style.display="none";
    icon2.value.style.display="block";
}

function placecontrol(event){
    const getpoint=event.offsetX
    const duration = thisvideo.value.duration;

    const currenttime=(getpoint/900)*duration;
    thisvideo.value.currentTime=currenttime
    thisvideo.value.play();
    if(currenttime>duration){currenttime=duration};
    
    thisvideo.value.ontimeupdate=function(){
        const currenttime=thisvideo.value.currentTime;
        const hour = parseInt(currenttime / 3600).toString().padStart(2,"0");
        const minute =parseInt(currenttime % 3600 / 60).toString().padStart(2,"0");
        const sec = parseInt(currenttime % 60).toString().padStart(2,"0");
        starttime.value=hour+':'+minute+':'+sec
    }

    const progress = (currenttime/duration) * 98.6;
    barwidth.value.style.width=progress+'%';
     
    icon1.value.style.display="none";
    icon2.value.style.display="block";
}

const voicenow1=ref();
const thisvoice=ref();

const screen=ref()


</script>

<template>
    <div>
        <input ref="inputfile" type="file" @change="playbutton">
        <div> 
            <video ref="thisvideo" type="video/mp4" :src="videofrom" width="900" height="600"></video>
        </div>
        <div>
            <div class="progressbar">
                <div class="border" @click="placecontrol"><div class="realline" ref="barwidth"></div></div>
            </div>
        </div>

        <div class="buttonplay">
            <button @click="playstop">
                <svg ref="icon1" t="1695540953771" class="icon" viewBox="0 0 1024 1024" version="1.1" xmlns="http://www.w3.org/2000/svg" p-id="1903" width="30" height="30" :style="{display:'block'}">
                    <path d="M830.577778 227.555556H657.066667l74.903703-70.162963c11.377778-11.377778 11.377778-29.392593 0-39.822223-5.688889-5.688889-13.274074-8.533333-21.807407-8.533333-7.585185 0-15.17037 2.844444-21.807407 8.533333L570.785185 227.555556H456.059259L338.488889 117.57037c-5.688889-5.688889-13.274074-8.533333-21.807408-8.533333-7.585185 0-15.17037 2.844444-21.807407 8.533333-11.377778 11.377778-11.377778 29.392593 0 39.822223L369.777778 227.555556H193.422222C117.57037 227.555556 56.888889 295.822222 56.888889 381.155556v332.8c0 85.333333 60.681481 153.6 136.533333 153.6h42.666667c0 25.6 22.755556 47.407407 50.251852 47.407407s50.251852-20.859259 50.251852-47.407407h353.659259c0 25.6 22.755556 47.407407 50.251852 47.407407s50.251852-20.859259 50.251852-47.407407h38.874074c75.851852 0 136.533333-69.214815 136.533333-153.6V381.155556c0.948148-85.333333-59.733333-153.6-135.585185-153.6zM698.785185 574.577778L425.718519 733.866667c-22.755556 13.274074-41.718519 2.844444-41.718519-24.651852V389.688889c0-26.548148 18.962963-37.925926 41.718519-24.651852l273.066666 160.237037c22.755556 14.222222 22.755556 35.081481 0 49.303704z" p-id="1904"></path>
                </svg>
                <svg ref="icon2" t="1695541092418" class="icon" viewBox="0 0 1024 1024" version="1.1" xmlns="http://www.w3.org/2000/svg" p-id="2061" width="30" height="30" :style="{display:'none'}">
                    <path d="M830.577778 227.555556H657.066667l74.903703-70.162963c4.740741-4.740741 8.533333-11.377778 8.533334-17.066667 0.948148-8.533333-1.896296-16.118519-8.533334-22.755556-5.688889-5.688889-13.274074-8.533333-21.807407-8.533333-7.585185 0-15.17037 2.844444-21.807407 8.533333L570.785185 227.555556H456.059259L338.488889 117.57037c-5.688889-5.688889-13.274074-8.533333-21.807408-8.533333-7.585185 0-15.17037 2.844444-21.807407 8.533333-11.377778 11.377778-11.377778 29.392593 0 39.822223L369.777778 227.555556H193.422222C117.57037 227.555556 56.888889 295.822222 56.888889 381.155556v332.8c0 85.333333 60.681481 153.6 136.533333 153.6h42.666667c0 25.6 22.755556 47.407407 50.251852 47.407407s50.251852-20.859259 50.251852-47.407407h353.659259c0 25.6 22.755556 47.407407 50.251852 47.407407s50.251852-20.859259 50.251852-47.407407h38.874074c75.851852 0 136.533333-69.214815 136.533333-153.6V381.155556c0.948148-85.333333-59.733333-153.6-135.585185-153.6zM455.111111 683.614815c0 25.6-20.859259 46.459259-47.407407 46.459259s-47.407407-20.859259-47.407408-46.459259V416.237037c0-25.6 20.859259-46.459259 47.407408-46.459259s47.407407 20.859259 47.407407 46.459259v267.377778z m208.592593 0c0 25.6-20.859259 46.459259-47.407408 46.459259s-47.407407-20.859259-47.407407-46.459259V416.237037c0-25.6 20.859259-46.459259 47.407407-46.459259s47.407407 20.859259 47.407408 46.459259v267.377778z" p-id="2062"></path>
                </svg>
            </button>
            <label>
                <span>{{starttime}}</span>
                <span>/</span>
                <span>{{endtime}}</span>
            </label>

            <span class="right">
                <button @click="godown1">+10</button>
                <button @click="godown2">-10</button>
                <span>
                    <span class="thisguy">
                        <button ref="thisvoice">
                            <svg t="1696325950901" class="icon" viewBox="0 0 1024 1024" version="1.1" xmlns="http://www.w3.org/2000/svg" p-id="3346" width="30" height="30">
                                <path d="M596 139c16.71 16.242 26 38.634 26 62v624c0 48.608-39.423 88-88 88-23.423 0-45.833-9.282-62-26L312.059 727.059A48 48 0 0 0 278.118 713H216c-70.657 0.295-128-57.003-128-128V441c0-70.338 57.343-127.636 128-128h61.198a48 48 0 0 0 33.844-13.962L472 139c34.021-34.576 89.774-34.604 124 0z m219.153 132.989C883.63 332.312 924 422.217 924 518.999c0 96.784-40.37 186.689-108.847 247.012-13.262 11.683-33.482 10.403-45.165-2.859-11.682-13.261-10.402-33.482 2.86-45.164C827.493 669.849 860 597.455 860 519s-32.507-150.85-87.152-198.988c-13.262-11.682-14.542-31.903-2.86-45.164 11.683-13.262 31.903-14.542 45.165-2.86z m-95.508 93.39C760.867 403.048 785 458.494 785 517.973c0 59.513-24.161 114.988-65.425 152.659-13.052 11.915-33.292 10.994-45.208-2.058-11.796-12.922-11.011-32.888 1.67-44.848l0.388-0.36 0.834-0.768C704.657 597.13 721 559.193 721 517.974c0-41.613-16.656-79.88-44.53-105.352-13.045-11.922-13.956-32.163-2.034-45.209 11.922-13.046 32.163-13.957 45.209-2.035z" fill="#333333" p-id="3347"></path>
                        </svg>
                        </button>
                        <div class="thisspeed" ref="voicenow1"><div class="voiceupdown"></div></div>
                    </span>
                </span>
                <button ref="screen">
                    <svg t="1696407738966" class="icon" viewBox="0 0 1024 1024" version="1.1" xmlns="http://www.w3.org/2000/svg" p-id="1555" width="30" height="30" :style="{display:'block'}">
                        <path d="M256 170.666667a128 128 0 0 0-128 128v213.333333a42.666667 42.666667 0 1 0 85.333333 0V298.666667a42.666667 42.666667 0 0 1 42.666667-42.666667h213.333333a42.666667 42.666667 0 1 0 0-85.333333H256z m512 682.666666a128 128 0 0 0 128-128v-170.666666a42.666667 42.666667 0 1 0-85.333333 0v170.666666a42.666667 42.666667 0 0 1-42.666667 42.666667h-192a42.666667 42.666667 0 1 0 0 85.333333H768z" fill="#14101C" p-id="1556"></path>
                    </svg>
                    <svg t="1696407784272" class="icon" viewBox="0 0 1024 1024" version="1.1" xmlns="http://www.w3.org/2000/svg" p-id="1707" width="200" height="200" :style="{display:'none'}">
                        <path d="M384 512a128 128 0 0 0 128-128V170.666667a42.666667 42.666667 0 1 0-85.333333 0v213.333333a42.666667 42.666667 0 0 1-42.666667 42.666667H170.666667a42.666667 42.666667 0 1 0 0 85.333333h213.333333z m277.333333 42.666667a128 128 0 0 0-128 128v170.666666a42.666667 42.666667 0 1 0 85.333334 0v-170.666666a42.666667 42.666667 0 0 1 42.666666-42.666667H853.333333a42.666667 42.666667 0 1 0 0-85.333333h-192z" fill="#14101C" p-id="1708"></path>
                    </svg>
                </button>
            </span>
        </div>
    </div>
    
    
</template>

<style scoped>
.border{
    cursor: pointer;
    border-style:solid;
    border-color: rgb(66, 103, 195);
    border-radius: 10px;
    border-width: 2px;
    width: 100%;
    height: 12px;
}
.realline{
    border-style:solid;
    border-color: rgb(66, 103, 195);
    border-radius: 5px;
    border-width: 5px;
    width: 0%;
    margin-top:1px;
    margin-left:1px;
}
.progressbar{
    margin-bottom: 2%;
}
.buttonplay{
   width: 100%;
   margin-right:80%;
}
.right{
   padding-left: 46%;
}
.thisspeed{
    cursor: pointer;
    border-style:solid;
    border-color: rgb(66, 103, 195);
    border-radius: 10px;
    border-width: 2px;
    width: 15%;
    height: 12px;
    transform: rotate(-90deg);
    z-index: 1;
    margin-left: 84%;
    margin-top: -15%;
    display: none;
}
.voiceupdown{
    border-style:solid;
    border-color: rgb(66, 103, 195);
    border-radius: 5px;
    border-width: 5px;
    width: 50%;
    margin-top:1px;
    margin-left:1px;
}
.thisguy:hover .thisspeed{
    display: block;
}

</style>
