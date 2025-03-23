<script setup>
import Map from "@/components/Map.vue";
import {ref} from "vue";

const isDark = ref(true);
const gradient = ref();
const theme = window.Telegram.WebApp.colorScheme;
if (theme == "dark") {
    isDark.value = true;
} else if (theme == "light") {
    isDark.value = false;
}

if (isDark.value) {
    gradient.value = "rgba(240,5,127,1) 0%, rgba(0,98,173,1) 50%, rgba(0,156,66,1) 100%";
} else {
    gradient.value = "#FB41A2 0%, #007FE0 50%, #01CB56 100%";
}

const PAIR_DURATION = 4800000; // В миллисекундах
const SATURDAY_PAIR_DURATION = 4500000;

const PAIRS_HOURSTART = [8, 10, 12, 13];
const PAIRS_MINSTART = [45, 25, 5, 35];
const BREAKS_HOURSTART = [8 ,10, 11, 13];
const BREAKS_MINSTART = [0 ,5, 45, 25];

const SATURDAY_PAIRS_HOURSTART = [8, 10, 11, 13];
const SATURDAY_PAIRS_MINSTART = [45, 10, 35, 0];
const SATURDAY_BREAKS_HOURSTART = [8 ,10, 11, 12];
const SATURDAY_BREAKS_MINSTART = [0 ,0, 25, 50];

let line = document.getElementById('progress_line');
let test = document.getElementById('test');
let player = document.getElementById('player');
let bellList = document.getElementById('bell_list');
let saturdayBellList = document.getElementById('saturday_bell_list');
let dummy = document.getElementById('dummy');

const timeLeft = ref("");
const timePassed = ref("");
const title = ref("\u200B");
const titleStart = ref("");
const titleEnd = ref("");

const progressWidth = ref(0);

let shootTime = false;

function padTo2(num) {
    if (num === 0) return '00'
    if (num>=10) return `${num}`
    return `0${num}`
}

function getTimeFormatted(date, beforeS){       // beforeS = 'Passed' || 'Left'
    date = new Date(Math.abs(date.getTime()));
    let hours = date.getUTCHours();
    let minutes = date.getUTCMinutes();
    let seconds = date.getUTCSeconds();
    let s = '';
    if (beforeS == 'Left' && seconds != 0 && minutes !=0) {
        minutes += 1;
    }
    if (minutes == 0 && hours == 0)
        s += `${seconds} сек.`;
    if (hours != 0) {
        s += `${hours} час`;
        if (minutes != 0) s += ' ';
    }
    if (minutes != 0) {
        if (minutes % 10 == 1 && (minutes/100).toFixed(1) != 0.1)
            s += `${minutes} минута`;
        else if ((minutes % 10 > 1 && minutes % 10 < 5) && (minutes/100).toFixed(1) != 0.1)
            s += `${minutes} минуты`;
        else
            s += `${minutes} минут`;
    }
    if (beforeS == 'Passed') {
        if (s.includes('час'))
            timePassed.value = 'Прошел '+s;
        else if (s.includes('минута'))
            timePassed.value = 'Прошла '+s;
        else if (s.includes('минут'))
            timePassed.value = 'Прошло '+s;
        else if (s.includes('сек.'))
            timePassed.value = 'Прошло '+s;
    }
    else if (beforeS == 'Left') {
        if (s.includes('час'))
            timeLeft.value = 'Остался '+s;
        else if (s.includes('минута'))
            timeLeft.value = 'Осталась '+s;
        else if (s.includes('минут'))
            timeLeft.value = 'Осталось '+s;
        else if (s.includes('сек.'))
            timeLeft.value = 'Осталось '+s;
    }
    // return `${padTo2(hours)}:${padTo2(minutes)}:${padTo2(seconds)}`;
}

function updatePairProgress(hourStart, minStart, duration) {
    let now = new Date();
    let start = new Date();
    start.setHours(hourStart, minStart, 0);
    let end = new Date();
    if (duration == PAIR_DURATION)
        end.setHours(start.getHours() + 1, start.getMinutes() + 20, 0);
    else
        end.setHours(start.getHours() + 1, start.getMinutes() + 15, 0);
    getTimeFormatted(new Date(start - now), 'Passed');
    getTimeFormatted(new Date(end - now), 'Left');
    // let progress = (Math.abs(start - now))/duration * 100;
    progressWidth.value = (Math.abs(start - now))/duration * 100;
}

function updateBreakProgress(hourBreakStart, minBreakStart, breakDuration) {        // breakDuration в минутах
    let now = new Date();
    let start = new Date();
    start.setHours(hourBreakStart, minBreakStart, 0);
    let end = new Date();
    end.setHours(start.getHours(), start.getMinutes() + breakDuration, 0);
    getTimeFormatted(new Date(start - now), 'Passed');
    getTimeFormatted(new Date(end - now), 'Left');
    // let progress = (Math.abs(start-now))/(breakDuration*60000)*100
    progressWidth.value = (Math.abs(start-now))/(breakDuration*60000)*100;
}

function getTime(PairHourArray, PairMinArray, breakHourArray, breakMinArray) {
    let now = new Date();
    let nowTime = (now.getHours()*60+now.getMinutes())*60000+now.getSeconds()*1000;
    if ( nowTime < 8*3600000  &&  nowTime > 5*3600000 ) {
        timePassed.value = 'Хорошего';
        timeLeft.value = 'дня!';
        progressWidth.value = 0;
    }
    for (let i = 0; i<4; i++) {
        let start = (PairHourArray[i]*60 + PairMinArray[i])*60000;
        if (PairHourArray != SATURDAY_PAIRS_HOURSTART)
            var end = start + PAIR_DURATION;
        else
            var end = start + SATURDAY_PAIR_DURATION;
        let startBreak = (breakHourArray[i]*60 + breakMinArray[i])*60000;

        if (i == 0) {
            var duration = 45;
        } else if (i < 3  &&  PairHourArray != SATURDAY_PAIRS_HOURSTART) {
            var duration = 20;
        } else {
            var duration = 10;
        }
        let endBreak = startBreak + duration*60000;

        if ( nowTime >= start && nowTime < end ) {
            let endHour = Math.floor(end/3600000);
            let endMin = (end/60000)-endHour*60;
            title.value = `${i+1} пара`;
            titleStart.value = `${PairHourArray[i]}:${padTo2(PairMinArray[i])}`;
            titleEnd.value = `${endHour}:${padTo2(endMin)}`;
            if (PairHourArray != SATURDAY_PAIRS_HOURSTART)
                updatePairProgress(PairHourArray[i], PairMinArray[i], PAIR_DURATION);
            else
                updatePairProgress(PairHourArray[i], PairMinArray[i], SATURDAY_PAIR_DURATION);
            return;

        } else if ( nowTime >= startBreak && nowTime < endBreak )  {
            if ( duration != 45 ) {
                if (progressWidth.value === 0) {
                    shootTime = true;
                }
                title.value = `Перерыв – ${duration} минут`;
            }
            else {
                title.value = `Перерыв`;
            }
            updateBreakProgress(breakHourArray[i], breakMinArray[i], duration);
            return;

        } else if (i == 3 && nowTime > (8*60+25)*60000) {
            title.value = 'Пары кончились'
            timePassed.value = 'Приятного';
            timeLeft.value = 'отдыха!';
            progressWidth.value = 100;
            shootTime = true;
        }
    }
}

const isSaturday = ref(false);
// function highlight(day = "default") {      // "default" | "saturday"
//     list.style.backgroundClip = 'padding-box';
//     list.style.border = '0.5vw solid transparent';
//     return;
// }

// First we check if you support touch, otherwise it's click:
let touchEvent = 'ontouchstart' in window ? 'touchstart' : 'click';

//
//      ИНТЕРВАЛЫ
//
let today = new Date().getDay();
if (today != 6) {
    let interval = setInterval(getTime, 1000, PAIRS_HOURSTART, PAIRS_MINSTART, BREAKS_HOURSTART, BREAKS_MINSTART);
    // highlight();
    isSaturday.value = false;
} else if (today == 6) {
    let interval = setInterval(getTime, 1000, SATURDAY_PAIRS_HOURSTART, SATURDAY_PAIRS_MINSTART, SATURDAY_BREAKS_HOURSTART, SATURDAY_BREAKS_MINSTART);
    // highlight();
    isSaturday.value = true;
}
// else if (today == 0) {
//     title.value = `Выходной`
//     timePassed.value = 'Приятного';
//     timeLeft.value = 'отдыха!';
//     progressWidth.value = 100;
// }
</script>

<template>
    <div class="container">
        <div id="progressBox">
            <div id="titleBox">
                <div id="title-start">{{ titleStart }}</div>
                <div id="title">{{ title }}</div>
                <div id="title-end">{{ titleEnd }}</div>
            </div>
            <div id="progress_bar">
                <div id="progress_line" :style="`width: ${progressWidth}%; --gradient: linear-gradient(90deg, ${gradient});`"></div>
                <div id="timeBox">
                    <div class="text" id="timerPassed" :hidden="title === `Перерыв`">{{ timePassed }}</div>
                    <div class="text" id="timerLeft">{{ timeLeft }}</div>
                </div>
            </div>
        </div>

        <div id="pairs_block">
                <div class="wrap list" id="bell_list" :style="isSaturday?`--border-color: var(--color-box-border)`:`color: var(--color-text); --border-color: linear-gradient(125deg, ${gradient});`">
                    <div class="row notlast">
                        <div>08:45</div>
                        <div>10:05</div>
                    </div>
                    <div class="row notlast">
                        <div>10:25</div>
                        <div>11:45</div>
                    </div>
                    <div class="row notlast">
                        <div>12:05</div>
                        <div>13:25</div>
                    </div>
                    <div class="row">
                        <div>13:35</div>
                        <div>14:55</div>
                    </div>
                </div>
                <div class="wrap list" id="saturday_bell_list" :style="!isSaturday?'--border-color: var(--color-box-border)':`color: var(--color-text); --border-color: linear-gradient(125deg, ${gradient});`">
                    <div class="row notlast">
                        <div>08:45</div>
                        <div>10:00</div>
                    </div>
                    <div class="row notlast">
                        <div>10:10</div>
                        <div>11:25</div>
                    </div>
                    <div class="row notlast">
                        <div>11:35</div>
                        <div>12:50</div>
                    </div>
                    <div class="row">
                        <div>13:00</div>
                        <div>14:15</div>
                    </div>
                </div>
        </div>
<!--        <footer>-->
<!--            <img id="dummy" src="/public/dummy.png" alt="">-->
<!--        </footer>-->
        <Map />
    </div>
</template>

<style scoped lang="scss">
.container {
    width: 100%;
    display: flex;
    //flex-grow: 0 1;
    flex-direction: column;
    gap: 1rem;
    position: relative;
}

.text {
    color: white;
    font-weight: bold;
}

#progressBox {
    display: flex;
    flex-direction: column;
    align-items: center;
}

#titleBox {
    width: 98%;
    display: flex;
    flex-direction: row;
    justify-content: space-between;
    align-items: flex-end;
    //font-size: 3.4vw;
    font-size: 1.5rem;
    color: var(--color-text);
}

#title {
    padding-bottom: .5rem;
    text-align: center;
    //font-size: 5vw;
    font-size: 2rem;
    font-weight: bold;
}

#timeBox {
    position: absolute;
    top: 0;
    //top: 50%;
    //left: 4%;
    //transform: translate(0px, -50%);
    display: flex;
    width: 100%;
    height: 100%;
    flex-direction: column;
    justify-content: center;
    align-items: start;
    margin-left: 1rem;
    //gap: .2rem;
}

#timerPassed {
    margin-bottom: -.4rem;
}

#timerLeft {
}

#progress_bar {
    position: relative;
    margin: 0 auto;
    border-radius: 1.6rem;
    width: 100%;
    height: 9.2rem;
    background-color:black;
    -webkit-mask: linear-gradient(#ffffff 0 0);
    mask: linear-gradient(#fff 0 0);
}

#progress_bar::before {
    content:"";
    position:absolute;
    top:0;
    left:0;
    right:0;
    bottom:0;
    background-image: linear-gradient(90deg, rgba(96,2,51,1) 0%, rgba(0,39,69,1) 50%, rgba(0,62,26,1) 100%);
}

#progress_line {
    //--gradient: linear-gradient(90deg, rgba(240,5,127,1) 0%, rgba(0,98,173,1) 50%, rgba(0,156,66,1) 100%);
    width: 0%;
    height: 100%;
    background-color: white;
    -webkit-mask: linear-gradient(#fff 0 0);
    mask:linear-gradient(#fff 0 0);
}

#progress_line::before {
    content:"";
    position:absolute;
    top:0;
    left:0;
    right:0;
    bottom:0;
    background-image: var(--gradient);
}

table, .list {
    .row {
        display: grid;
        grid-template-columns: repeat(2, 1fr);
        padding: .5rem 0;
    }
    --border: .1rem;
    position: relative;
    width: 100%;
    color: var(--color-inactive-text);
    font-size: 2rem;
    text-align: center;
    border: var(--border) solid transparent !important;
    background-clip: padding-box;
    //border-collapse: collapse;
}

table::after {
    content: '';
    position: absolute;
    top: calc(var(--border) * -1); bottom: calc(var(--border) * -1);
    left: calc(var(--border) * -1); right: calc(var(--border) * -1);
    background: var(--border-color);
    z-index: -1;
    border-radius: 1rem;
}

.notlast {
    position: relative;
}

.notlast::after {
    content: "";
    position: absolute;
    display: block;
    bottom: 0;
    left: 1rem;
    right: 1rem;
    //width: calc(100% - 2rem);
    height: 0.08rem;
    background-color: var(--color-box-border);
}

td {
}

//.notlast>td {
//}

#pairs_block {
    display: flex;
    justify-content: center;
    gap: 1rem;
    z-index: 2;
}

footer {
    text-align: center;
    margin-top: auto;
}

img {
    width: 15vw;
    margin: auto;
}

@media (max-width: 400px) {
    .list {
        font-size: 1.5rem;
    }

    #titleBox {
        font-size: 1.2rem;
    }

    #title {
        font-size: 1.5rem;
    }
}

@media (max-width: 350px) {
    .list {
        font-size: 1.2rem;
    }

    #titleBox {
        font-size: 1rem;
    }

    #title {
        font-size: 1.2rem;
    }
}
</style>