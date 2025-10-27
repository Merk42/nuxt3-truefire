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

const lessonlayout = computed(() => {
    return `view-${view.value}`
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
            <div id="upper-filters">
                <input type="search" v-model="fterm" placeholder="search" aria-label="search"/>
                <select aria-label="sort">
                    <option>Sort</option>
                </select>
                <fieldset>
                    <legend>View</legend>
                    <input id="view-1" type="radio" name="view" v-model="view" :value="1"/>
                    <label for="view-1">
                        <UIcon name="i-lucide-lightbulb" class="size-5" />
                    </label>
                    <input id="view-2" type="radio" name="view" v-model="view" :value="2"/>
                    <label for="view-2">
                        <UIcon name="i-lucide-lightbulb" class="size-5" />
                    </label>
                    <input id="view-3" type="radio" name="view" v-model="view" :value="3"/>
                    <label for="view-3">
                        <UIcon name="i-lucide-lightbulb" class="size-5" />
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
<style scoped>
#lesson-grid {
    height:100vh;
}

main {
    margin-inline:.5rem
}
aside {
    display: none;
}
aside ul+ul {
    border-top: 1px solid #314b65;
}
#view-3,
#view-3+label {
    display: none;
}
@media only screen and (min-width:640px) {
    aside{
        display: grid;
    }
    main {
        margin-inline:3rem
    }
    
    #view-3,
    #view-3+label{
        display: block;
    }

}
#result-summary,
#result-summary span{
    display:flex;
    align-items: center;
    color:var(--secondary-copy)
}
#result-summary span+span::before {
    content:"\A";
    width:2px;
    aspect-ratio:1;
    border-radius:50%;
    background: var(--secondary-copy);
    display:inline-block;
    margin:0 10px;
}
#lessons-list {
    display: grid;
    gap:1rem;
    --_across:1;
    grid-template-columns: repeat(var(--_across), minmax(0, 1fr)); 
}
#lessons-list.view-2 {
    --_across:2
}
#lessons-list.view-3 {
    --_across:3
}

#upper-filters {
    display: flex;
    gap:.5rem;
    margin-bottom: 1.5rem;
}

fieldset{
    display: flex;
    border: none;
    align-items: center;
    padding:0;
    flex-grow: 1;
    justify-content: right;
}
fieldset legend  {
    float: left;
}
input[type="radio"] {
    	width:0;
	opacity:0;
	position:absolute;
}
input[type="radio"]+label{
    cursor: pointer;
    border:1px solid transparent;
    border-radius: 100vw;
    padding: .5rem;
    aspect-ratio: 1;
    display:inline-block
}
input[type="radio"]:checked+label{
    border-color: black;
}

input[type="search"],
select {
    border-radius: 100vw;
    height:35px;
}
select {
    flex-basis: max-content;
}
input[type="search"] {
    background-color: var(--inactive-color);
    border-color:transparent;
    padding-left:1rem
}
</style>