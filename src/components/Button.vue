<template>
    <v-btn class="button d-flex align-center" :height="small ? 57 : 119" :class="{ small }"
        >{{ text }} <template v-slot:append><IconRightArrow v-if="icon" class="ml-4" /></template
    ></v-btn>
</template>

<script setup>
import IconRightArrow from "@/components/icons/IconArrow.vue";

defineProps({
    text: String,
    icon: Boolean,
    small: Boolean,
});
</script>

<style scoped>
.button {
    padding: 0 50px;
    border-radius: 88px;
    border: 7px solid #fff;
    font-family: "Luckiest Guy", cursive;
    font-size: 40px;
    letter-spacing: 0.01em;
    color: #ffffff;
    background: linear-gradient(180deg, #e9a32c 0%, #de631e 100%);
}

.small {
    padding: 0 38px;
    border: none;
    font-size: 24px;
}
</style>
