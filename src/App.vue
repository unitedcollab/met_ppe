<template>
    <v-app>
        <v-main class="main">
            <Home v-if="screen === 'home'" @start="startGameHandler" />
            <Game v-else-if="screen === 'game'" v-model:screen="screen" />
            <End v-else-if="screen === 'end'" @play-again="playAgainHandler" />
        </v-main>
    </v-app>
</template>

<script setup>
import { ref } from "vue";
import Home from "@/components/Home.vue";
import End from "@/components/End.vue";
import Game from "@/components/Game.vue";

const screen = ref("home");

const startGameHandler = () => {
    screen.value = "game";
};

const playAgainHandler = () => {
    screen.value = "home";
    window.top.postMessage({ completed: false }, "*");
};
</script>

<style>
html {
    overflow-y: auto;
}

.main {
    font-family: "Montserrat SB", sans-serif;
    font-size: 24px;
    line-height: normal;
    color: #fff;
    background: linear-gradient(0deg, #00c4ff -46.56%, #015ea2 98.34%);
}

@font-face {
    font-family: "Montserrat";
    src: url("./assets/fonts/Montserrat-Regular.ttf") format("truetype");
    font-weight: 400;
    font-style: normal;
    font-display: swap;
}

@font-face {
    font-family: "Montserrat SB";
    src: url("./assets/fonts/Montserrat-SemiBold.ttf") format("truetype");
    font-weight: 600;
    font-style: normal;
    font-display: swap;
}

@font-face {
    font-family: "Montserrat B";
    src: url("./assets/fonts/Montserrat-Bold.ttf") format("truetype");
    font-weight: 700;
    font-style: normal;
    font-display: swap;
}

@font-face {
    font-family: "Luckiest Guy";
    src: url("./assets/fonts/LuckiestGuy-Regular.ttf") format("truetype");
    font-weight: 400;
    font-style: normal;
    font-display: swap;
    ascent-override: 100%;
}
</style>
