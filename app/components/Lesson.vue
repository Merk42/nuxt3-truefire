<script setup lang="ts">
import type { PERSONAL_RESULT } from '../type';
const props = defineProps<{
  lesson: PERSONAL_RESULT;
}>();

const totaltime = computed(() => {
    const MIN = Math.floor(props.lesson.duration / 60)
    const SEC = props.lesson.duration % 60;
    return `${MIN}:${SEC < 10 ? '0'+SEC : SEC}`
})
</script>
<template>
    <div class="lesson_wrapper @container">
        <div class="grid gap-1 p-1 @[50vw]:grid-cols-[auto_1fr_auto] border-1 border-solid rounded-sm">
            <img alt="img name" :src="lesson.Image" class="rounded-sm overflow-hidden w-full @[50vw]:h-[50px]"/>
            <span class="itemTitle" v-html="lesson.Name"></span>
            <span class="flex gap-1 items-center">
                <UProgress class="@[50vw]:col-span-3 @[50vw]:order-1"  v-model="lesson.progress" :max="lesson.duration" />
                <span class="hidden md:block">{{ totaltime }}</span>
            </span>
            
            <span class="text-right">
                <UIcon :name="lesson.bookmarked ? 'fa7-solid:bookmark' : 'fa7-regular:bookmark'" class="size-5" />
                <UIcon :name="lesson.progress >= lesson.duration ? 'fa7-solid:play-circle' : 'fa7-regular:play-circle'" class="size-5" />
            </span>
        </div>
    </div>
</template>
