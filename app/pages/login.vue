<script setup lang="ts">
import { computed, ref } from 'vue';
const username = ref("");
const password = ref("")

const cancontinue = computed(() => {
    return !!username.value && !!password.value
})

const handleSubmit = async () => {
    navigateTo('/overview')
}
</script>
<template>
    <div id="sign-in-page" class="md:grid md:grid-cols-[400px_1fr]">
        <div id="effect" class="hidden md:block p-4">
            <h1>Welcome Back</h1>
        </div>
        <div id="form-cell" class="flex justify-center">
            <div id="form-container">
                <h2>Sign in to ArtistWorks</h2>
                <p>New user? <a href="#">Create an account.</a></p>
                <form @submit.prevent="handleSubmit">
                    <UFormField label="Enter username">
                        <UInput v-model="username" />
                    </UFormField>
                    <UFormField label="Enter password">
                        <UInput v-model="password" />
                    </UFormField>
                    <UButton :disabled="!cancontinue">Continue</UButton>
                </form>
                <p><a href="#">Forgot password?</a></p>

                <span class="hr-span">Or sign up with</span>
                <!-- GOOGLE | APPLE -->
            </div>
        </div>
    </div>
</template>