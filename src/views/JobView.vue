<script setup>
import BackButton from '@/components/BackButton.vue';
import axios from 'axios';
import { reactive, onMounted, ref } from 'vue';
import { useToast } from 'vue-toastification';
import { RouterLink, useRoute, useRouter } from 'vue-router';
import PulseLoader from 'vue-spinner/src/PulseLoader.vue';

// const state = reactive({
//     isLoading: true,
//     job: {},
//     company: {}
// })

const job = ref({})
const company = ref({})
const isLoading = ref(true)

const route = useRoute();
const toast = useToast();
const router = useRouter();
const jobID = route.params.id;

onMounted(async () => {
    try {
        const response = await fetch(`/api/jobs/${jobID}`);
        const data = await response.json();
        job.value = data
        company.value = data.company
        // state.job = data;
        // state.company = data.company;
    } catch (err) {
        console.error("Error Fetching job data...", err)
    } finally {
        setTimeout(() => {
            // state.isLoading = false
            isLoading.value = false
        }, 2000);
    }
})

const handleDelete = async () => {
    try {
        const res = await axios.delete(`/api/jobs/${jobID}`);
        toast.success("Job Deleted Successfully!")
        router.push("/jobs")
    } catch (err) {
        console.error("Error Deleteing Job!", err)
        toast.success("Error Deleteing Job!")
    }
}
</script>

<template>
    <section v-if="!isLoading" class="bg-green-50">
        <BackButton />
        <div class="container m-auto py-10 px-6">
            <div class="grid grid-cols-2 md:grid-cols-70/30 w-full gap-6">
                <main>
                    <div class="bg-white p-6 rounded-lg shadow-md text-center md:text-left">
                        <div class="text-gray-500 mb-4">{{ job.type }}</div>
                        <h1 class="text-3xl font-bold mb-4">{{ job.title }}</h1>
                        <div class="text-gray-500 mb-4 flex align-middle justify-center md:justify-start">
                            <i class="pi pi-map-marker text-2xl text-orange-700 mr-2"></i>
                            <p class="text-orange-700">{{ job.location }}</p>
                        </div>
                    </div>

                    <div class="bg-white p-6 rounded-lg shadow-md mt-6">
                        <h3 class="text-green-800 text-lg font-bold mb-6">
                            Job Description
                        </h3>

                        <p class="mb-4">
                            {{ job.description }}
                        </p>

                        <h3 class="text-green-800 text-lg font-bold mb-2">Salary</h3>

                        <p class="mb-4">{{ job.salary }} / Year</p>
                    </div>
                </main>

                <!-- Sidebar -->
                <aside>
                    <!-- Company Info -->
                    <div class="bg-white p-6 rounded-lg shadow-md">
                        <h3 class="text-xl font-bold mb-6">Company Info</h3>

                        <h2 class="text-2xl">{{ company?.name }}</h2>

                        <p class="my-2">
                            {{ company?.description }}
                        </p>

                        <hr class="my-4" />

                        <h3 class="text-xl">Contact Email:</h3>

                        <p class="my-2 bg-green-100 p-2 font-bold">
                            {{ company?.contactEmail }}
                        </p>

                        <h3 class="text-xl">Contact Phone:</h3>

                        <p class="my-2 bg-green-100 p-2 font-bold">
                            {{ company?.contactPhone }}
                        </p>
                    </div>

                    <!-- Manage -->
                    <div class="bg-white p-6 rounded-lg shadow-md mt-6">
                        <h3 class="text-xl font-bold mb-6">Manage Job</h3>
                        <RouterLink :to="`/jobs/${job.id}/edit`"
                            class="bg-green-500 hover:bg-green-600 text-white text-center font-bold py-2 px-4 rounded-full w-full focus:outline-none focus:shadow-outline mt-4 block">
                            Edit
                            Job
                        </RouterLink>
                        <button @click="handleDelete"
                            class="bg-red-500 hover:bg-red-600 text-white font-bold py-2 px-4 rounded-full w-full focus:outline-none focus:shadow-outline mt-4 block">
                            Delete Job
                        </button>
                    </div>
                </aside>
            </div>
        </div>
    </section>
    <div v-else class="w-full flex justify-center items-center my-20">
        <PulseLoader />
    </div>
</template>
