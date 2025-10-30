<script setup lang="ts">

import { ref, computed } from 'vue';

import type { LESSONS_RESPONSE, PERONAL_DATA } from '../type';

const data = ref<LESSONS_RESPONSE|null>(null);
const personal = ref<PERONAL_DATA|null>(null);

const numtopics = ref(64);
const numlesson = ref(284);
const numexchanges = ref(1673)
const view = ref(1);
const fterm = ref('');

const sortOptions = ref(['sort'])
const sortValue = ref('sort')

const lessonlayout = computed(() => {
    return `grid grid-cols-${view.value} gap-4`
})

async function fetchAllData() {
  const personalPromise:Promise<PERONAL_DATA> = fetch('/personal.json').then(response => response.json());
  const generalPromise:Promise<LESSONS_RESPONSE> = fetch('/lessons.json').then(response => response.json());
  

  try {
    const [data1, data2] = await Promise.all([personalPromise, generalPromise]);
    console.log('All data fetched successfully:', data1, data2);
    personal.value = data1;
    data.value = data2;
  } catch (error) {
    console.error('Error fetching data:', error);
  }
}

const filteredLessons = computed(() => {
    const P:PERONAL_DATA|null = personal.value
    if (!data.value?.search.lessons?.result.results) {
        return []
    }
    
    const GENERAL = !!fterm.value ? data.value?.search.lessons?.result.results.filter(result => result.Name.toLowerCase().includes(fterm.value.toLowerCase())) : data.value?.search.lessons?.result.results
    if (!GENERAL) {
        return []
    }
    const M = GENERAL.map(result => ({
        ...result,
        bookmarked:P && P[result.id.toString()]?.bookmarked ? true : false,
        progress:P && P[result.id.toString()]?.progress ? P[result.id.toString()].progress ?? 0 : 0
    }));
    return M
})

fetchAllData()
</script>

<template>
    <div id="lesson-grid" class="grid gap-4 md:grid-cols-[300px_1fr] md:grid-rows-[50px_1fr] 
                
                
                md:[grid-template-areas:'aside_header'_'aside_main']">
        <header class="[grid-area:header]"></header>
        <Sidebar></Sidebar>
        <main class="[grid-area:main]">
            <h1>Intermediate</h1>
            <p id="result-summary">
                <span>{{numtopics}} Topics</span>
                <span>{{numlesson}} Lesson Videos</span>
                <span>{{numexchanges}} Video Exchanges</span>
            </p>
            <div id="upper-filters" class="flex">
                <UInput icon="fa7-regular:circle" size="xl" variant="outline" placeholder="search"  v-model="fterm" />
                <USelect v-model="sortValue" :items="sortOptions" class="w-48" />
                <fieldset>
                    <legend>View</legend>
                    <input id="view-1" type="radio" name="view" v-model="view" :value="1"/>
                    <label for="view-1">
                        <UIcon name="fa7-solid:grip-lines" class="size-5" />
                    </label>
                    <input id="view-2" type="radio" name="view" v-model="view" :value="2"/>
                    <label for="view-2">
                        <UIcon name="fa7-solid:grip-vertical" class="size-5" />
                    </label>
                    <input id="view-3" type="radio" name="view" v-model="view" :value="3"/>
                    <label for="view-3">
                        <UIcon name="fa7-solid:grip-horizontal" class="size-5" />
                    </label>
                </fieldset>
            </div>
            <div id="lessons-list" :class="lessonlayout">
                <p v-if="filteredLessons.length === 0">No lessons match that filter</p> 
                <template v-for="result in filteredLessons" :key="result.id">
                    <Lesson :lesson="result" />
                </template>
            </div>
        </main>
    </div>
</template>
