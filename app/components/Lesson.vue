<script setup lang="ts">
import type { PERSONAL_RESULT } from '../type';
const props = defineProps<{
  lesson: PERSONAL_RESULT;
}>();
</script>
<template>
    <div class="lesson_wrapper @container">
        <div class="grid gap-1 p-1 @[50vw]:grid-cols-[auto_1fr_auto] border-1 border-solid rounded-sm">
            <img alt="img name" :src="lesson.Image" class="rounded-sm overflow-hidden w-full @[50vw]:h-[50px]"/>
            <span class="itemTitle" v-html="lesson.Name"></span>
            <UProgress class="@[50vw]:col-span-3 @[50vw]:order-1"  v-model="lesson.progress" :max="lesson.duration" />
            <span class="text-right">
                <UIcon name="i-lucide-lightbulb" class="size-5" />
                <UIcon name="i-lucide-lightbulb" class="size-5" />
            </span>
        </div>
    </div>
</template>
