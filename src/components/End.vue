<template>
    <v-col class="d-flex flex-column align-center justify-center fill-height">
        <v-container class="container d-flex flex-column align-center justify-center py-15 w-100">
            <v-img :src="logo" :max-width="624" width="100%" class="mb-14" />
            <p class="text text-center">Stay alert, stay safe, and remember: Safety Takes Everyone!</p>
            <Button text="Play again" @click="$emit('play-again')" />
        </v-container>
    </v-col>
</template>

<script setup>
import Button from "@/components/Button.vue";
import logo from "@/assets/images/game-over.png";

defineEmits(["play-again"]);
</script>

<style scoped>
.container {
    max-width: 624px;
}

.text {
    margin-bottom: 86px;
    font-family: "Montserrat B", sans-serif;
    font-size: 32px;
}
</style>
