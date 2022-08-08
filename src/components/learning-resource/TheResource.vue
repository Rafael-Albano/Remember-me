<template>
    <base-card>
        <base-button @click="setSelectedTab('stored-resources')" :mode="storedResButtonMode">
            Stored Resources
        </base-button>
        <base-button @click="setSelectedTab('add-resources')" :mode="addResButtonMode">
            Add Resource
        </base-button>
    </base-card>
    <keep-alive>
        <component :is="selectedTab"></component>
    </keep-alive>
</template>

<script>
import { v4 as uuidv4 } from 'uuid';
import StoredResources from './StoredResources.vue'
import AddResources from './AddResources.vue'

export default {
    components: {
        StoredResources,
        AddResources
    },

    name: 'the-resource',
    data() {
        return {
            selectedTab: 'stored-resources',
            storedResources: [
                {
                    id: uuidv4(),
                    title: "Official Guide",
                    description: "The official Vue.js documentation.",
                    link: "https://vuejs.org/"
                },
                {
                    id: uuidv4(),
                    title: "Google",
                    description: "Learn to google...",
                    link: "https://www.google.com.br/"
                },
            ]
        };
    },

    provide() {
        return {
            resources: this.storedResources,
            addResource: this.addResource,
            removeResource: this.removeResource
        }
    },

    computed: {
        storedResButtonMode() {
            return this.selectedTab === 'stored-resources' ? null : 'flat'
        },
        addResButtonMode() {
            return this.selectedTab === 'add-resources' ? null : 'flat'
        }
    },

    methods: {
        setSelectedTab(tab) {
            this.selectedTab = tab;
        },
        addResource(title, description, link) {
            const newResource = {
                id: uuidv4(),
                title,
                description,
                link
            }

            this.storedResources.unshift(newResource);
            this.selectedTab = 'stored-resources';
            
        },
        removeResource(id) {
            const index = this.storedResources.findIndex(res => res.id === id);
            this.storedResources.splice(index, 1);
        }
    }
}
</script>