<script setup lang="ts">
import { computed, ref } from 'vue';
import * as z from 'zod'
import type { FormSubmitEvent } from '@nuxt/ui'

const schema = z.object({
  username:  z.string().min(1, 'Must be at least 1 character'),
  password: z.string().min(8, 'Must be at least 8 characters')
})

type Schema = z.output<typeof schema>

const state = reactive<Partial<Schema>>({
  username: undefined,
  password: undefined
})

async function onSubmit(event: FormSubmitEvent<Schema>) {
    console.log('onsubmit fired...')
    navigateTo('/overview')
}

const cancontinue = computed(() => {
    return !!state.username && !!state.password
})

</script>
<template>
    <div id="sign-in-page" class="md:grid md:grid-cols-[400px_1fr] h-screen">
        <div id="effect" class="hidden md:block p-4 bg-blue-600">
            <h1 class="text-5xl text-white">Welcome Back</h1>
        </div>
        <div id="form-cell" class="flex justify-center">
            <div id="form-container" class="self-center">
                <h2 class="text-3xl">Sign in to ArtistWorks</h2>
                <p>New user? <a href="#">Create an account.</a></p>
                <UForm :schema="schema" :state="state" class="space-y-4" @submit="onSubmit">
                    <UFormField label="Enter username" size="xl">
                        <UInput v-model="state.username" />
                    </UFormField>
                    <UFormField label="Enter password" size="xl">
                        <UInput v-model="state.password" type="password"/>
                    </UFormField>
                    <UButton type="submit" size="xl" class="w-full rounded-full text-center justify-center">Continue</UButton>
                </UForm>
                <p><a href="#">Forgot password?</a></p>

                <span class="hr-span">Or sign up with</span>
                <!-- GOOGLE | APPLE -->
            </div>
        </div>
    </div>
</template>