<template>
    <v-col class="d-flex align-center justify-center fill-height pa-0">
        <v-container class="left-container d-flex flex-column align-center ga-9 h-100">
            <div>
                <div class="scenario-count mb-3">
                    <span class="text-uppercase">Scenario {{ scenario }}</span> of {{ scenariosNumber }}
                </div>
                <p>{{ scenarios[scenario].text }}</p>
            </div>
            <Sortable :key="scenario" :list="cards" item-key="placed" tag="div" :options="options" class="cards ga-5">
                <template #item="{ element }">
                    <v-col
                        v-if="element.scenario.includes(scenario)"
                        :data-id="element.id"
                        :key="element.id"
                        class="card d-flex align-center pa-5"
                        :class="{ placed: placedItemsIds.includes(element.id) }"
                    >
                        <v-img :src="element.image" :width="100" />
                    </v-col>
                </template>
            </Sortable>
            <Button v-if="allPlaced" text="Next" small @click="nextScenarioHandler" />
        </v-container>
        <v-container class="position-relative d-flex justify-center">
            <div class="position-relative d-flex justify-center">
                <Sortable :options="options" :list="cards" item-key="id" tag="img" :src="geraldinoImg" @add="placeCardHandler" class="main-image" draggable="false"> </Sortable>
                <template v-for="card in cards" :key="card.id">
                    <Sortable
                        v-if="card.finalImage && placedItemsIds.includes(card.id)"
                        :key="card.id"
                        :options="options"
                        class="position-absolute d-none"
                        :class="{ 'd-block': placedItemsIds.includes(card.id) }"
                        :list="cards"
                        item-key="id"
                        tag="img"
                        :src="card.finalImage"
                        :style="{
                            top: card.position.y + 'px',
                            left: card.position.x + 'px',
                            zIndex: card.zIndex || 0,
                        }"
                        @add="placeCardHandler"
                        draggable="false"
                    >
                    </Sortable>
                </template>
            </div>
            <AnswerMark v-if="showMark" :correct="draggingItem?.correct?.includes(scenario)" class="mark position-absolute" />
        </v-container>
    </v-col>
</template>

<script setup>
import { computed, ref } from "vue";
import { Sortable } from "sortablejs-vue3";
import Button from "@/components/Button.vue";
import AnswerMark from "@/components/AnswerMark.vue";
import geraldinoImg from "@/assets/images/Geraldino.svg";
import card1 from "@/assets/cards/vest.png";
import card2 from "@/assets/cards/scrigit.png";
import card3 from "@/assets/cards/goggles.png";
import card4 from "@/assets/cards/harness.png";
import card5 from "@/assets/cards/mallet.png";
import card6 from "@/assets/cards/knife.png";
import card7 from "@/assets/cards/headphones.png";
import card8 from "@/assets/cards/reach-tool.png";
import card9 from "@/assets/cards/spotter.png";
import card10 from "@/assets/cards/spotter-flag.png";
import card11 from "@/assets/cards/beam-buddy.png";
import card12 from "@/assets/cards/barricade.png";
import card13 from "@/assets/cards/gloves.png";
import finalCard1 from "@/assets/cards/correct/gloves.svg";
import finalCard2 from "@/assets/cards/correct/reach-tool.svg";
import finalCard3 from "@/assets/cards/correct/scrigit.svg";
import finalCard4 from "@/assets/cards/correct/beam-buddy.svg";
import finalCard5 from "@/assets/cards/correct/mallet.svg";
import finalCard6 from "@/assets/cards/correct/spotter-flag.svg";
import finalCard7 from "@/assets/cards/correct/spotter.svg";
import finalCard8 from "@/assets/cards/correct/barricade.svg";
import finalCard9 from "@/assets/cards/correct/harness.svg";
import correctSnd from "@/assets/audios/Correct.wav";
import incorrectSnd from "@/assets/audios/Incorrect.wav";
import { arraysAreEqual } from "@/utilities";

const scenarios = {
    1: {
        text: "You are on a ladder trying to get a box that is out of reach. Select the right PPE & tool(s) for the job.",
        items: [8, 13],
    },
    2: {
        text: "You are replacing price labels on a beam. Select the right PPE & tool(s) for the job.",
        items: [2, 13],
    },
    3: {
        text: "You and an associate are adjusting a beam. Select the right PPE & tool(s) for the job.",
        items: [5, 11, 13],
    },
    4: {
        text: "While the store is open, you are retrieving product from the overhead using an Order Picker. Select the right PPE & tool(s) for the job.",
        items: [4, 8, 9, 10, 12, 13],
    },
    5: {
        text: "Your associate asks you to spot for them while using a Reach Truck. Select the right PPE & tool(s) for the job.",
        items: [10, 12, 13],
    },
};

const emits = defineEmits(["update:screen"]);

const audio = new Audio();

const scenario = ref(1);
const showMark = ref(false);
const draggingItem = ref(null);
const placedItemsIds = ref([]);
const cards = ref([
    { id: 1, image: card1, scenario: [1, 2] },
    { id: 2, image: card2, scenario: [2, 3, 4, 5], correct: [2], finalImage: finalCard3, position: { y: 395, x: 13 }, placed: false },
    { id: 3, image: card3, scenario: [1, 2, 3, 4, 5] },
    { id: 4, image: card4, scenario: [1, 2, 3, 4, 5], correct: [4], finalImage: finalCard9, position: { y: 134, x: -49 }, zIndex: 2, placed: false },
    { id: 5, image: card5, scenario: [1, 2, 3, 4, 5], correct: [3], finalImage: finalCard5, position: { y: 379, x: 213 }, placed: false },
    { id: 6, image: card6, scenario: [1, 2, 3, 4, 5] },
    { id: 7, image: card7, scenario: [1, 2, 3, 4, 5] },
    { id: 8, image: card8, scenario: [1, 2, 3, 4, 5], correct: [1, 4], finalImage: finalCard2, position: { y: 339, x: 121 }, zIndex: 2, placed: false },
    { id: 9, image: card9, scenario: [1, 2, 3, 4, 5], correct: [4], finalImage: finalCard7, position: { y: 8, x: -40 }, placed: false },
    { id: 10, image: card10, scenario: [1, 2, 3, 4, 5], correct: [4, 5], finalImage: finalCard6, position: { y: 152, x: -100 }, placed: false },
    { id: 11, image: card11, scenario: [1, 2, 3, 4, 5], correct: [3], finalImage: finalCard4, position: { y: 379, x: -22 }, placed: false },
    { id: 12, image: card12, scenario: [1, 3, 4, 5], correct: [4, 5], finalImage: finalCard8, position: { y: 78, x: 280 }, placed: false },
    { id: 13, image: card13, scenario: [1, 2, 3, 4, 5], correct: [1, 2, 3, 4, 5], finalImage: finalCard1, position: { y: 373, x: 0 }, zIndex: 2, placed: false },
]);
const options = ref({
    group: { name: "shared", pull: "clone", push: false },
    animation: 150,
    ghostClass: "ghost",
    dragClass: "drag",
    list: cards.value,
    sort: false,
    filter: ".placed",
    onMove: (event) => {
        return !event.related.classList.contains("placed");
    },
});
const scenariosNumber = computed(() => Object.keys(scenarios).length);
const allPlaced = computed(() => arraysAreEqual(placedItemsIds.value, scenarios[scenario.value].items));

const playAudio = (source) => {
    audio.src = source;
    audio.play();
};

const showMarkHandler = () => {
    showMark.value = true;
    setTimeout(() => {
        showMark.value = false;
    }, 1000);
};

const placeCardHandler = (evt) => {
    draggingItem.value = cards.value.find((card) => card.id === +evt.clone.dataset.id);
    showMarkHandler();
    if (draggingItem.value.correct?.includes(scenario.value)) {
        draggingItem.value.placed = true;
        placedItemsIds.value.push(draggingItem.value.id);
        playAudio(correctSnd);
    } else {
        playAudio(incorrectSnd);
    }
};

const nextScenarioHandler = () => {
    if (scenario.value === scenariosNumber.value) {
        emits("update:screen", "end");
        scenario.value = 1;
        window.top.postMessage({ completed: true }, "*");
    } else {
        scenario.value++;
    }
    placedItemsIds.value = [];
    draggingItem.value = null;
    cards.value = cards.value.map((card) => ({ ...card, placed: false }));
};
</script>

<style scoped>
.left-container {
    width: 53.125%;
    padding: 60px 70px;
    background: #0f0f0f;
}

.scenario-count {
    font-family: "Montserrat", sans-serif;
    font-size: 16px;
    color: #cfcfcf;
}

.scenario-count > span {
    color: #e9a32c;
    font-family: "Montserrat B", sans-serif;
}

.cards {
    display: grid;
    grid-template-columns: repeat(4, 1fr);
}

.card {
    max-width: 120px;
    max-height: 120px;
    border-radius: 10px;
    background: rgba(255, 255, 255, 0.3);
}

.placed {
    opacity: 0.5;
}

.main-image {
    z-index: 1;
}

.mark {
    top: 50%;
    left: 50%;
    transform: translate3d(-50%, -50%, 0);
}
</style>
