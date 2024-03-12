<template>
    <v-col class="d-flex flex-column align-center justify-center fill-height">
        <v-container class="container d-flex flex-column align-center justify-center py-15 w-100">
            <v-img :src="logo" :max-width="489" width="100%" class="mb-14" />
            <p class="text text-center">Help Gerald prepare for a safe day by matching the correct PPE items with corresponding body parts for each scenario!</p>
            <Button text="Start" icon @click="$emit('start')" />
        </v-container>
    </v-col>
</template>

<script setup>
import Button from "@/components/Button.vue";
import logo from "@/assets/images/dd-ppe.png";

defineEmits(["start"]);
</script>

<style scoped>
.container {
    max-width: 624px;
}

.text {
    margin-bottom: 76px;
}
</style>
