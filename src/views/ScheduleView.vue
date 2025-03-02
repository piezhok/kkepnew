<script setup>

</script>

<template>
    <div class="container">
        <div id="progressBox">
            <div id="titleBox">
                <div id="title-start">&nbsp</div>
                <div id="title">&nbsp</div>
                <div id="title-end">&nbsp</div>
            </div>
            <div id="progress_bar">
                <div id="progress_line"></div>
                <div id="timeBox">
                    <div class="text" id="timerPassed"></div>
                    <div class="text" id="timerLeft"></div>
                </div>
            </div>
        </div>

        <div id="pairs_block">
            <table id="bell_list">
                <tr class="notlast">
                    <td>08:45</td>
                    <td>-</td>
                    <td>10:05</td>
                </tr>
                <tr class="notlast">
                    <td>10:25</td>
                    <td>-</td>
                    <td>11:45</td>
                </tr>
                <tr class="notlast">
                    <td>12:05</td>
                    <td>-</td>
                    <td>13:25</td>
                </tr>
                <tr>
                    <td>13:35</td>
                    <td>-</td>
                    <td>14:55</td>
                </tr>
            </table>
            <table id="saturday_bell_list">
                <tr class="notlast">
                    <td>08:45</td>
                    <td>-</td>
                    <td>10:00</td>
                </tr>
                <tr class="notlast">
                    <td>10:10</td>
                    <td>-</td>
                    <td>11:25</td>
                </tr>
                <tr class="notlast">
                    <td>11:35</td>
                    <td>-</td>
                    <td>12:50</td>
                </tr>
                <tr>
                    <td>13:00</td>
                    <td>-</td>
                    <td>14:15</td>
                </tr>
            </table>
        </div>

        <div id="emoji_placeholder">
            <tgs-player autoplay loop mode="normal" src="" class="emoji-player" id="emoji">
            </tgs-player>
            <!-- <dotlottie-player class="emoji-player" id="player" src="" background="transparent" speed="1" direction="1" mode="normal" loop autoplay mute>
            </dotlottie-player> -->
            <!-- <video playsinline muted autoplay loop src="Stickers/Break.mov" type="video/mov" id="player">
            </video> -->
        </div>
        <footer>
            <img id="dummy" src="/public/dummy.png" alt="">
        </footer>
    </div>
</template>

<style scoped>
.container{
    margin: 0;
    display: flex;
    flex-direction: column;
    min-height: 100vh;
}

.text{
    color: white;
    font-weight: bold;
    font-size: 4vw;
}

#progressBox {
    /* background-color: var(--tg-theme-bg-color); */
    /* height: 38vw; */
    /* border-radius: 0 0 5vw 5vw; */
}

#titleBox {
    width: 86%;
    margin: auto;
    margin-top: .5vw;
    display: flex;
    flex-direction: row;
    justify-content: space-between;
    align-items: flex-end;
    font-size: 3.4vw;
    color: var(--color-text);
    /* background-color: red; */
    /* justify-content: space-evenly; */
}

#title {
    /* position: absolute; */
    /* top: -25%; */
    /* margin-top: 2px;
    margin-bottom: 4px; */
    /* margin-top: 0.5vw; */
    padding: .5vw;
    text-align: center;
    font-size: 5vw;
}

#timeBox {
    position: absolute;
    top: 50%;
    left: 4%;
    transform: translate(0px, -50%);
}

#timerPassed {
    margin-top: 0vw;
    /* margin-top: 0px; */
}

#timerLeft {
    margin-top: 0.5vw;
    /* margin-top: 5px; */
}

#progress_bar {
    position: relative;
    margin: 0 auto;
    border-radius: 6vw;
    width: 96%;
    height: 25vw;
    background-color: black;
    -webkit-mask:linear-gradient(#fff 0 0);
    mask:linear-gradient(#fff 0 0);
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
    width: 0%;
    height: 100%;
    background-color: white;
    -webkit-mask:linear-gradient(#fff 0 0);
    mask:linear-gradient(#fff 0 0);
}

#progress_line::before {
    content:"";
    position:absolute;
    top:0;
    left:0;
    right:0;
    bottom:0;
    background-image: linear-gradient(90deg, rgba(240,5,127,1) 0%, rgba(0,98,173,1) 50%, rgba(0,156,66,1) 100%);
}

.emoji-player {
    /* width: 50%; */
    width: 25vw;
    /* width: 10rem; */
    height: 0%;
    max-width: 500px;
    margin: 0 auto;
}

#emoji_placeholder{
    margin: -1vw auto;
    width: 100%;
    /* margin-top: vw; */
    text-align: center;
}

p {
    /* position: absolute; */
    color: var(--tg-theme-hint-color);
    /* position: absolute; */
    /* margin: auto; */
    /* top:0%; */
    /* margin-right: 2%; */
    /* bottom: 0.2vw; */
    text-align: center;
    margin: auto;
    /* margin-top: ;
    margin-left: 5vw; */
    font-size: 2vw;
    width: 100vw;
    /* font-size: 90%; */
    /* max-width: 70%; */
}

a {
    color: var(--tg-theme-link-color);
    text-decoration: none;
}

table {
    position: relative;
    margin: 4vw 2vw;
    width: 10vw;
    /* height: 40vw;
    width: 70vw; */
    /* height: 40vw;
    width: 45vw; */
    /* height: 30vw;
    width: 35vw; */
    background-color: var(--tg-theme-bg-color);
    color: var(--tg-theme-text-color);
    /* padding: 2vw 3vw; */
    padding: 3vw 4vw;
    font-size: 5vw;
    text-align: center;
    /* word-spacing: 1vw; */
    border-radius: 4vw;
    border: 0.7vw solid var(--tg-theme-hint-color);
    /* background-clip: padding-box; */
}

table::after {
    position: absolute;
    top: -0.5vw; bottom: -0.5vw;
    left: -0.5vw; right: -0.5vw;
    background: linear-gradient(125deg, rgba(240,5,127,1) 0%, rgba(0,98,173,1) 50%, rgba(0,156,66,1) 100%);
    content: '';
    z-index: -1;
    border-radius: 4vw;
}

.notlast>td {
    padding-bottom: 2vw;
}

#pairs_block {
    display: -webkit-box;
    justify-content: center;
    -webkit-box-pack: center;
}

footer {
    text-align: center;
    margin-top: auto;
}

img {
    width: 15vw;
    margin: auto;
}
</style>