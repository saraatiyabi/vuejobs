<script setup>
import { ref, defineProps, onMounted, reactive } from 'vue';
import axios from 'axios';
import PulseLoader from 'vue-spinner/src/PulseLoader.vue'
import JobListing from './JobListing.vue';

// const jobs = ref([])
const state = reactive({
    jobs: [],
    isLoading: true
})

const props = defineProps({
    limit: Number,
    showButton: {
        type: Boolean,
        default: false
    }
})

onMounted(async () => {
    try {
        const response = await axios.get("/api/jobs");
        state.jobs = response.data;
    } catch (err) {
        console.log("Error Fetching jobs...", err)
    }
    finally {
        setTimeout(() => {
            state.isLoading = false
        }, 2000);
    }
})
</script>

<template>
    <section class="bg-green-50 px-4 py-10">
        <div class="container-xl lg:container m-auto">
            <h2 class="text-3xl font-bold text-green-500 mb-6 text-center">
                Browse Jobs
            </h2>
            <!-- Show Pulse Spinner while Loading -->
            <div v-if="state.isLoading" class="text-center text-gray-500 mb-5">
                <PulseLoader />
            </div>
            <!-- Show Job Listings While Done Loading -->
            <div v-else class="grid grid-cols-1 md:grid-cols-3 gap-6">
                <JobListing v-for="job in state.jobs.slice(0, limit || state.jobs.length)" :key="job.id" :job="job" />
            </div>
        </div>
    </section>

    <section v-if="showButton" class="m-auto max-w-lg my-10 px-6">
        <RouterLink to="/jobs" class="block bg-black text-white text-center py-4 px-6 rounded-xl hover:bg-gray-700">View
            All Jobs
        </RouterLink>
    </section>
</template>
