<template>
    <div class="mark d-flex align-center justify-center" :class="correct ? 'correct' : 'wrong'">
        <IconCorrect v-if="correct" />
        <IconWrong v-else />
    </div>
</template>

<script setup>
import IconCorrect from "@/components/icons/IconCorrect.vue";
import IconWrong from "@/components/icons/IconWrong.vue";

defineProps({
    correct: Boolean,
});
</script>

<style scoped>
.mark {
    width: 170px;
    height: 170px;
    border-radius: 50%;
    z-index: 3;
}

.correct {
    background: #0a8700;
}

.wrong {
    background: #ed1c24;
}
</style>
