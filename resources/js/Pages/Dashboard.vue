<script setup>
import AuthenticatedLayout from '@/Layouts/AuthenticatedLayout.vue';
import { Head } from '@inertiajs/vue3';
import { ref, watch } from 'vue';
import axios from 'axios';

const search = ref('');
const results = ref([]);
// const users = ref([]);

defineProps({
    users: Array,
});


async function getData() {
   try {
        const response = await axios.get(route('search'), {
            params: { query: search.value },
        });
         console.log(response.data);
         results.value = response.data;
    } catch (error) {
        console.error('Error fetching search results:', error);
    }
}


</script>

<template>
    <Head title="Dashboard" />

    <AuthenticatedLayout>
        <template #header>
            <h2 class="font-semibold text-xl text-gray-800 leading-tight">Dashboard</h2>
        </template>

        <div class="py-12">
            <div class="max-w-7xl mx-auto sm:px-6 lg:px-8">
                <div class="bg-white overflow-hidden shadow-sm sm:rounded-lg">

                    <div>
                        <form @submit.prevent class="mb-8">
                            <label for="search" class="mr-4">Search</label>
                            <input type="text" id="search"
                            @input="getData"
                            v-model="search" class="border rounded p-1" />

                            <div v-if="search">
                                <p><strong>Search Message:</strong> {{ search }}</p>
                            </div>
                        </form>
                        <ul v-if="results.length">
                            <li v-for="user in results" :key="user.id">
                                {{ user.id }} : {{ user.name }} :  {{ user.email }}
                            </li>
                        </ul>
                        <p v-else > No Result is found! </p>


                        <div class="mt-16">
                            <ul>
                            <li v-for="user in users" :key="user.id">
                                {{ user.id }} : {{ user.name }} :  {{ user.email }}
                            </li>
                        </ul>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </AuthenticatedLayout>
</template>
