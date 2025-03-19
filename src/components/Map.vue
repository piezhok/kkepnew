<script setup>
import {onMounted, ref, watch} from "vue";
    const search = defineModel("search");
    const buildings = defineModel("buildings");
    const floors = defineModel("floors");

    const floorQuantity = ref(1);
    const themeVal = ref("dark");

    const theme = window.Telegram.WebApp.colorScheme;
    if (theme == "dark") {
        themeVal.value = "dark";
    } else if (theme == "light") {
        themeVal.value = "light";
    }

    onMounted(() => {
        const markers = {
            "0-1": [
                {"loc": [-48.224673, 95.976563], "title": "108"},
                {"loc": [-39.368279, 95.976563], "title": "109"},
                {"loc": [-21.943046, 95.976563], "title": "110"},
                {"loc": [-10.833306, 96.328125], "title": "111"},
                {"loc": [13.581921, 95.625000], "title": "112"},
                {"loc": [31.052934, 95.976563], "title": "113"},
                {"loc": [46.073231, 95.625000], "title": "114"},
                {"loc": [57.891497, 95.625000], "title": "115"},
                {"loc": [66.652977, 95.625000], "title": "116"},
                {"loc": [-73.726595, 95.976563], "title": "120"},
                {"loc": [-63.074866, 95.976563], "title": "121"},
                {"loc": [-35.460670, 34.453125], "title": "общежитие"},
                {"loc": [46.558860, -90.000000], "title": "спортзал"},
                {"loc": [83.236426, -4.570313], "title": "тир"}
            ],
            "1-1": [
                {"loc": [60.586967, -43.593750], "title": "м мужской туалет"},
                {"loc": [60.239811, 14.765625], "title": "14"},
                {"loc": [60.586967, 123.750000], "title": "15"},
                {"loc": [60.586967, 152.578125], "title": "16"},
                {"loc": [-33.137551, 154.687500], "title": "19"},
                {"loc": [-1.406109, -253.828125], "title": "актовый зал"},
                {"loc": [-9.102097, 244.687500], "title": "иц информационный центр"}
            ],
            "1-2": [
                {"loc": [65.802776, -59.765625], "title": "ж женский туалет"},
                {"loc": [-61.100789, 134.296875], "title": "20а"},
                {"loc": [-61.100789, 75.234375], "title": "21"},
                {"loc": [-61.100789, 0.000000], "title": "22"},
                {"loc": [-61.438767, -74.531250], "title": "23"},
                {"loc": [-61.773123, -130.078125], "title": "24"},
                {"loc": [-61.100789, -228.515625], "title": "25"},
                {"loc": [9.449062, -248.203125], "title": "26"},
                {"loc": [66.089364, -228.515625], "title": "27"},
                {"loc": [65.512963, -12.656250], "title": "28"},
                {"loc": [65.512963, 26.718750], "title": "29"},
                {"loc": [65.802776, 69.609375], "title": "30"},
                {"loc": [-3.864255, 227.812500], "title": "читальный зал 20"}
            ],
            "1-3": [
                {"loc": [67.067433, 188.437500], "title": "31"},
                {"loc": [66.791909, 254.531250], "title": "32"},
                {"loc": [9.795678, 266.484375], "title": "33"},
                {"loc": [-62.267923, 214.453125], "title": "34"},
                {"loc": [-62.593341, 73.828125], "title": "35"},
                {"loc": [-62.267923, -30.234375], "title": "36"},
                {"loc": [-62.593341, -122.343750], "title": "37"},
                {"loc": [-62.915233, -226.406250], "title": "38"},
                {"loc": [9.795678, -249.609375], "title": "39"},
                {"loc": [66.513260, -227.812500], "title": "40"},
                {"loc": [67.067433, -61.875000], "title": "41"},
                {"loc": [66.791909, 1.406250], "title": "42"},
                {"loc": [66.791909, 66.796875], "title": "43"}
            ],
            "1-4": [
                {"loc": [66.791909, 226.406250], "title": "44"},
                {"loc": [-61.938950, 211.640625], "title": "45"},
                {"loc": [-62.593341, 76.640625], "title": "46"},
                {"loc": [-61.938950, -29.531250], "title": "47"},
                {"loc": [-62.267923, -118.125000], "title": "48"},
                {"loc": [-62.267923, -227.812500], "title": "49"},
                {"loc": [11.178402, -248.906250], "title": "50"},
                {"loc": [66.791909, -228.515625], "title": "51"},
                {"loc": [66.791909, -57.656250], "title": "52"},
                {"loc": [66.513260, 6.328125], "title": "53"},
                {"loc": [66.513260, 68.203125], "title": "54"}
            ],
            "2-1": [
                {"loc": [-37.718590, -166.640625], "title": "86"},
                {"loc": [-72.181804, -225.000000], "title": "87"},
                {"loc": [-14.604847, -271.406250], "title": "88"},
                {"loc": [65.072130, -246.796875], "title": "89"},
                {"loc": [69.411242, -68.906250], "title": "м мужской туалет"},
                {"loc": [69.657086, -22.500000], "title": "ж женский туалет"},
                {"loc": [-7.710992, 143.437500], "title": "столовая"}
            ],
            "2-2": [
                {"loc": [63.548552, -147.656250], "title": "91"},
                {"loc": [80.760615, -63.984375], "title": "92"},
                {"loc": [73.428424, 7.734375], "title": "93"},
                {"loc": [64.774125, 120.937500], "title": "вц1 вц 1 вычислительный центр 1"},
                {"loc": [-66.231457, 88.593750], "title": "95"},
                {"loc": [-74.211983, -0.703125], "title": "вц2 вц 2 вычислительный центр 2"},
                {"loc": [-73.627789, -113.203125], "title": "вц3 вц 3 вычислительный центр 3"},
                {"loc": [-71.965388, -225.000000], "title": "98"},
                {"loc": [-17.308688, -250.312500], "title": "99"},
                {"loc": [35.460670, -251.015625], "title": "100"},
                {"loc": [-66.231457, 156.796875], "title": "104"},
                {"loc": [65.946472, 236.250000], "title": "105"},
                {"loc": [-36.597889, 248.203125], "title": "106"}
            ],
            "2-3": [
                {"loc": [42.163403, -39.902344], "title": "101"},
                {"loc": [-73.540855, -124.409180], "title": "102"}
            ],
            "3-1": [
                {"loc": [56.944974, -163.828125], "title": "м мужской туалет"},
                {"loc": [-53.330873, -254.531250], "title": "55"},
                {"loc": [-53.330873, -180.000000], "title": "56"},
                {"loc": [4.214943, -296.015625], "title": "57"},
                {"loc": [56.944974, -239.062500], "title": "58"},
                {"loc": [56.944974, -82.968750], "title": "59"},
                {"loc": [56.944974, 6.328125], "title": "60"},
                {"loc": [56.944974, 118.828125], "title": "61"},
                {"loc": [56.944974, 174.375000], "title": "62"},
                {"loc": [56.559482, 252.421875], "title": "63"},
                {"loc": [3.513421, 267.187500], "title": "64"},
                {"loc": [-52.908902, 232.734375], "title": "65"},
                {"loc": [-53.330873, 135.000000], "title": "66"},
                {"loc": [-53.330873, 38.671875], "title": "68"},
                {"loc": [-53.330873, -20.390625], "title": "69"},
                {"loc": [-53.330873, -78.750000], "title": "70"}
            ],
            "3-2": [
                {"loc": [54.572062, -162.421875], "title": "ж женский туалет"},
                {"loc": [54.572062, -255.234375], "title": "72"},
                {"loc": [54.977614, -68.203125], "title": "73"},
                {"loc": [54.572062, 21.093750], "title": "74"},
                {"loc": [54.572062, 118.828125], "title": "75"},
                {"loc": [54.162434, 213.046875], "title": "76"},
                {"loc": [-0.703107, 264.375000], "title": "77"},
                {"loc": [-55.379110, 232.734375], "title": "78"},
                {"loc": [-54.977614, 136.406250], "title": "79"},
                {"loc": [-55.379110, 51.328125], "title": "80"},
                {"loc": [-55.776573, -19.687500], "title": "81"},
                {"loc": [-55.379110, -80.859375], "title": "82"},
                {"loc": [-55.379110, -137.812500], "title": "83"},
                {"loc": [-55.379110, -184.921875], "title": "84"},
                {"loc": [-55.379110, -250.312500], "title": "85"}
            ]
        };
        floors.value = 1;

        let map = L.map('map-container', { attributionControl:false, zoomControl: false }).setView([0, 0], 0.05);
        let img = L.imageOverlay(`maps/${themeVal.value}/0-1.svg`, [[90, -110],[-90, 110]]).addTo(map);

        const setMap = (file) => {
            img.remove();
            img = L.imageOverlay(`maps/${themeVal.value}/${file}.svg`, [[-100,-300],[100, 300]]).addTo(map);
        }

        let markersLayer = new L.LayerGroup();
        map.addLayer(markersLayer);

        const setMarkers = (data) => {
            markersLayer.remove();
            markersLayer = new L.LayerGroup();	//layer contain searched elements
            map.addLayer(markersLayer);
            for(let i = 0; i < markers[data].length; i++) {
                let title = markers[data][i].title,	//value searched
                    loc = markers[data][i].loc,		//position found
                    marker = new L.Marker(loc, {title: title} );//se property searched
                markersLayer.addLayer(marker);
            }
        }

        watch(buildings, (val) => {
            if (val == "0") {
                floors.value = 1;
                floorQuantity.value = 1;
            } else if (val == "1") {
                floors.value = 1;
                floorQuantity.value = 4;
            } else if (val == "2") {
                floors.value = 1;
                floorQuantity.value = 3;
            } else if (val == "3") {
                floors.value = 1;
                floorQuantity.value = 2;
            }
            setMap(`${val}-1`);
            setMarkers(`${val}-1`);
        });
        watch(floors, (val) => {
            setMap(`${buildings.value}-${val}`);
            setMarkers(`${buildings.value}-${val}`);
        });
        let typingTimer = null;
        watch(search, (val) => {
            clearTimeout(typingTimer);
            typingTimer = setTimeout(() => {
                if (val === "") {
                    return;
                } else if ((val.toLowerCase() === "м" || val.toLowerCase() === "ж") && buildings.value != 0) {
                    let toilet = val.toLowerCase() === "м" ? "мужской" : "женский";
                    for (let i = 1; i <= floorQuantity.value; i++) {
                        markers[`${buildings.value}-${i}`].forEach((element) => {
                            if (element.title.includes(toilet)) {
                                floors.value = i;
                                map.flyTo(element.loc, 1, { duration: .5 });
                            }
                        })
                    }
                } else {
                    let shouldBreak = false;
                    for (const key of Object.keys(markers)) {
                    // Object.keys(markers).forEach((key) => {
                        markers[key].forEach((element) => {
                            if ((/[а-яА-Я]/.test(val) && element.title.includes(val.toLowerCase()) || (!/[а-яА-Я]/.test(val) && element.title === val.toLowerCase()))) {
                                buildings.value = key[0];
                                setTimeout(() => floors.value = key[2], 0);
                                map.flyTo(element.loc, 1, { duration: .5 });
                                shouldBreak = true;
                            }
                        })
                        if (shouldBreak) {break;}
                    }
                }
            }, 500);
        });
    })


    // const setBuilding = (newVal) => {
    //     if (newVal == "territory") {
    //         floors.value = 1;
    //         floorQuantity.value = 1;
    //     } else if (newVal == "k1") {
    //         floors.value = 1;
    //         floorQuantity.value = 4;
    //     } else if (newVal == "k2") {
    //         floors.value = 1;
    //         floorQuantity.value = 3;
    //     } else if (newVal == "k3") {
    //         floors.value = 1;
    //         floorQuantity.value = 2;
    //     }
    // }
</script>

<template>
    <div class="map">
        <div class="wrap input-container">
            <div class="search-box">
                <input v-model="search" type="text" name="search" id="search" placeholder="Поиск">
            </div>
            <div class="buildings-box">
                <span>
                    <input v-model="buildings" checked type="radio" name="building" value=0 id="building-territory">
                    <label for="building-territory">Территория</label>
                </span>

                <span>
                    <input v-model="buildings" type="radio" name="building" value=1 id="building-k1">
                    <label for="building-k1">Корпус 1</label>
                </span>

                <span>
                    <input v-model="buildings" type="radio" name="building" value=2 id="building-k2">
                    <label for="building-k2">Корпус 2</label>
                </span>

                <span>
                    <input v-model="buildings" type="radio" name="building" value=3 id="building-k3">
                    <label for="building-k3">Корпус 3</label>
                </span>

            </div>
            <div class="floors-box">
                <span v-for="n in floorQuantity" :key="n">
                    <input :checked="n==floors" v-model="floors" type="radio" name="floors" :value="`${n}`" :id="`floor${n}`">
                    <label :for="`floor${n}`">Этаж {{ n }}</label>
                </span>
            </div>
        </div>
        <div class="wrap" id="map-container">
        </div>
    </div>
</template>

<style scoped lang="scss">
    .map {
        width: 100%;
        height: 100%;
        display: flex;
        flex-direction: column;
        gap: 1rem;
        flex-grow: 1;
    }

    .input-container {
        display: flex;
        flex-direction: column;
        align-items: center;
        padding: 1.2rem .8rem;
        background-color: var(--color-box);
        color: var(--color-inactive-text);
        width: 100%;
        gap: 1.2rem;

        div {
            width: 100%;
            display: flex;
            flex-direction: row;
            align-items: center;
            justify-content: space-evenly;
            font-size: 1.4rem;
            position: relative;
        }
    }

    .buildings-box {
        margin-bottom: .6rem;
    }

    .floors-box {
        margin-top: .6rem;
    }

    .buildings-box::after {
        content: "";
        display: block;
        position: absolute;
        bottom: -1.2rem;
        width: 100%;
        height: 0.08rem;
        background-color: var(--color-box-border);
    }

    input[type="radio"] {
        all: unset;
        display: none;
        -webkit-tap-highlight-color: rgba(0, 0, 0, 0);
    }

    input[type="text"] {
        all: unset;
        width: 100%;
        background-color: var(--color-box-border);
        color: var(--color-text);
        padding: .8rem 1.2rem;
        border-radius: 1rem;
        font-size: 1.6rem;
        -webkit-tap-highlight-color: rgba(0, 0, 0, 0);
    }

    input[type="text"]::placeholder {
        color: var(--color-inactive-text);
    }

    label {
        -webkit-tap-highlight-color: rgba(0, 0, 0, 0);
        all: unset;
        padding: 0 .8rem;
        text-align: center;
        width: 100%;
        transition: all .25s;
        white-space: nowrap;
        cursor: pointer;
    }

    input[type="radio"]:checked+label {
        font-weight: 600;
        color: var(--color-text);
    }

    #map-container {
        width: 100%;
        height: 100%;
        flex-grow: 1;
    }

    @media (max-width: 400px) {
        .input-container {
            div {
                font-size: 1.2rem;
            }
        }
    }

    @media (max-width: 350px) {
        .input-container {
            div {
                font-size: 1rem;
            }
        }
    }
</style>