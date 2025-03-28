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
              
                        <div class="md:col-span-5">
                            <label for="url" class="text-sm font-medium text-indigo-600 mb-2">URL</label>
                            <input id="url" v-model="newLink.url" placeholder="Enter URL..." required 
                            class="w-full px-4 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-indigo-500 focus:outline-none" />
                        </div>

                        <div class="md:col-span-3">
                            <label for="category" class="text-sm font-medium text-indigo-600 mb-2">Category</label>
                            <select id="category" v-model="newLink.category" required
                            class="w-full px-4 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-indigo-500 focus:outline-none">
                                <option value="WATCH">WATCH</option>
                                <option value="READ">READ</option>
                            </select>
                        </div>
                    </div>
            
                    <div class="flex justify-center">
                        <button type="submit" class="px-6 py-2 bg-indigo-600 text-white rounded-lg hover:bg-indigo-700 transition-colors flex items-center space-x-2" >
                            <span>+</span>
                            <span>Add Link</span>
                        </button>
                    </div>
                </form>

                <!-- Links List-->
                <div class="mt-8 space-y-4">
                    <div v-for="link in links" :key="link.id" 
                    class="bg-white/80 backdrop-blur-lg rounded-xl shadow-md p-4 flex flex-col hover:bg-white/90 transition-all duration-300">
                        <div class="flex justify-between items-center mb-1">
                            <h3 class="text-xl font-bold text-indigo-800">{{ link.title }}</h3>
                            <span :class="{
                            'bg-purple-600': link.category === 'WATCH', 
                            'bg-green-600': link.category === 'READ' }" 
                            class="text-white px-2 py-1 rounded-md text-xs uppercase">{{ link.category }}</span>
                        </div>
                        <a :href="link.url" target="_blank" 
                        class="text-blue-600 hover:text-blue-800 hover:underline">{{ link.url }}</a>
                    </div>
                </div>
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
        category: 'WATCH' | 'READ'
        order?: number
    }
  
    const newLink = ref({
        title: '',
        url: '',
        category: 'WATCH' as 'WATCH' | 'READ'
    })

    // Link array - will store all added links
    const links = ref<Link[]>([])
  
    // Function adding a new link
    const addLink = () => {
        // Prevent adding empty links
        if (!newLink.value.title.trim() || !newLink.value.url.trim()) return

        // Add https:// if not present
        if (!newLink.value.url.startsWith('http://') && !newLink.value.url.startsWith('https://')) {
            newLink.value.url = 'https://' + newLink.value.url
        }
    
        // Creates a new link object
        const link: Link = {
            ...newLink.value,
            id: Date.now(),
            order: links.value.length + 1
        }
       
        // Adds a new link to the links array
        links.value.push(link)   
    
        // Resets the form to empty values
        newLink.value = {
            title: '',
            url: '',
            category: 'WATCH'
        }
    }
</script>