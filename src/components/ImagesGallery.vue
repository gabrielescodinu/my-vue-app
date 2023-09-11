<template>
    <div class="py-8 text-center">
        <div class="grid grid-cols-4 gap-4 max-w-6xl mx-auto">
            <div class="relative group" v-for="image in images" :key="image.id">
                <img class="w-full h-full object-cover rounded-md cursor-pointer" :src="image.src.medium" :alt="image.id" @click="openFullscreen(image)" />
                <p class="absolute bottom-0 bg-white/50 hidden group-hover:block w-full p-2">{{ image.photographer }}</p>
            </div>
        </div>
        <div class="my-8 space-x-4">
            <button class="bg-gray-600 hover:bg-gray-400 p-4 rounded-md text-white" @click="goToPreviousPage" :disabled="currentPage === 1">Precedente</button>
            <button class="bg-gray-600 hover:bg-gray-400 p-4 rounded-md text-white" @click="goToNextPage" :disabled="currentPage === totalPages">Successivo</button>
        </div>
    </div>
    <div v-if="fullscreenImage" class="fullscreen fixed top-0 left-0 w-full h-full z-50 bg-black-100 flex flex-col justify-center" @click="closeFullscreen">
        <img :src="fullscreenImage.src.medium" :alt="fullscreenImage.id" />
    </div>
</template>

<script>
import axios from 'axios'

export default {
    data() {
        return {
            images: [],
            fullscreenImage: null,
            currentPage: 1,
            totalPages: 0
        }
    },
    mounted() {
        this.fetchImages()
    },
    methods: {
        fetchImages() {
            axios.get(`https://api.pexels.com/v1/curated?page=${this.currentPage}&per_page=12`, {
                headers: {
                    Authorization: 'ewDZ4s3dpci2ZaeCRqNyuifprkuV5PA9stMOLIyolRvlyAKwmW9g5Z67'
                }
            })
                .then(response => {
                    console.log(response.data.photos)
                    this.images = response.data.photos
                    this.totalPages = Math.ceil(response.data.total_results / 10)
                })
                .catch(error => {
                    console.log('Errore:', error)
                })
        },
        goToNextPage() {
            if (this.currentPage < this.totalPages) {
                this.currentPage++
                this.fetchImages()
            }
        },
        goToPreviousPage() {
            if (this.currentPage > 1) {
                this.currentPage--
                this.fetchImages()
            }
        },
        openFullscreen(image) {
            this.fullscreenImage = image
        },
        closeFullscreen() {
            this.fullscreenImage = null
        },
    }
}
</script>
