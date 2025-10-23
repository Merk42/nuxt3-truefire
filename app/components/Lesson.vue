<script setup lang="ts">
import type { PERSONAL_RESULT } from '../type';
import ProgressDisplay from './ProgressDisplay.vue';
const props = defineProps<{
  lesson: PERSONAL_RESULT;
}>();
</script>
<template>
    <div class="lesson_wrapper">
        <div class="lesson">
            <img alt="img name" :src="lesson.Image"/>
            <span class="itemTitle" v-html="lesson.Name"></span>
            <ProgressDisplay class="progress" :total="lesson.duration" :completed="lesson.progress"></ProgressDisplay>
            <span class="status">
                <font-awesome icon="bookmark" :class="{ 'active': lesson.bookmarked }" />
                <font-awesome icon="circle-check" :class="{ 'active': lesson.progress >= lesson.duration }" />
            </span>
        </div>
    </div>
</template>
<style scoped>
.active{
    color:var(--progress-blue)
}
.lesson {
    display: grid;
    gap:.75rem;
    padding:.5rem;
    border:1px solid var(--border-color);
    border-radius:1rem;
}
img {
    width:100%;
    border-radius: .5rem;
    overflow: hidden;
}
small {
    color:var(--secondary-copy)
}
.status {
    text-align: right;
}

.lesson_wrapper {
  container-type: inline-size;
}


@container (width > 50vw) {
    .lesson img {
        height:50px
    }
    .lesson {
        grid-template-columns: auto 1fr auto;
    }
    .lesson .progress {
        order:1;
        grid-column: span 3;
    }
}
</style>