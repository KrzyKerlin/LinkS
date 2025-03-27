<template>
    <div class="min-h-screen bg-gradient-to-br from-purple-500 to-blue-500 py-10">
        <div class="container mx-auto max-w-4xl px-4">
            <div class="bg-white/80 backdrop-blur-lg rounded-2xl shadow-lg p-6">
                <h1 class="text-5xl font-extrabold text-center mb-8 bg-gradient-to-r from-indigo-900 to-purple-400 bg-clip-text text-transparent leading-tight">
                My LinkS
                </h1>
          
                <form @submit.prevent="addLink" class="space-y-4">
                    <div class="grid grid-cols-1 md:grid-cols-12 gap-4">
                        <div class="md:col-span-4">
                            <label for="title" class="text-sm font-medium text-indigo-600 mb-2">Title</label>
                            <input id="title" v-model="newLink.title" placeholder="Enter title..." required 
                            class="w-full px-4 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-indigo-500 focus:outline-none" />
                        </div>
              
                        <div class="md:col-span-8">
                            <label for="url" class="text-sm font-medium text-indigo-600 mb-2">URL</label>
                            <input id="url" v-model="newLink.url" placeholder="Enter URL..." required 
                            class="w-full px-4 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-indigo-500 focus:outline-none" />
                        </div>
                    </div>
            
                    <div class="flex justify-center">
                        <button type="submit" class="px-6 py-2 bg-indigo-600 text-white rounded-lg hover:bg-indigo-700 transition-colors flex items-center space-x-2" >
                            <span>+</span>
                            <span>Add Link</span>
                        </button>
                    </div>
                </form>
            </div>
        </div>
    </div>
</template>
  
<script setup lang="ts">
    import { ref } from 'vue'
  
    interface Link {
        id: number
        title: string
        url: string
        order?: number
    }
  
    const newLink = ref({
        title: '',
        url: ''
    })
  
    const links = ref<Link[]>([])
  
    const addLink = () => {
        if (!newLink.value.url.startsWith('http://') && !newLink.value.url.startsWith('https://')) {
        newLink.value.url = 'https://' + newLink.value.url
        }
    
        const link: Link = {
            ...newLink.value,
            id: Date.now(),
            order: links.value.length + 1
        }
    
        links.value.push(link)
    
        newLink.value = {
            title: '',
            url: ''
        }
    }
</script>
