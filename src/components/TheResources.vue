<template>
  <base-card>
    <base-button
      @click="setSelectedTab('stored-resources')"
      :mode="selectedTab === 'stored-resources' ? null : 'flat'"
    >
      Stored Resources
    </base-button>

    <base-button
      @click="setSelectedTab('add-resource')"
      :mode="selectedTab === 'add-resource' ? null : 'flat'"
    >
      Add Resource
    </base-button>
  </base-card>
  <keep-alive>
    <component :is="selectedTab"></component>
  </keep-alive>
</template>

<script>
import StoredResources from '@/components/StoredResources.vue';
import AddResource from '@/components/AddResource.vue';

export default {
  components: {
    StoredResources,
    AddResource,
  },
  data() {
    return {
      selectedTab: 'stored-resources',
      storedResources: [
        {
          id: 'official-guide',
          title: 'Official Guide',
          description: 'The official Vue.js documentation.',
          link: 'https://vuejs.org',
        },
        {
          id: 'google',
          title: 'Google',
          description: 'Learn to google...',
          link: 'https://www.google.com',
        },
      ],
    };
  },
  provide() {
    return {
      resources: this.storedResources,
      addResource: this.addResource,
      deleteResource: this.deleteResource,
    };
  },
  methods: {
    setSelectedTab(tab) {
      this.selectedTab = tab;
    },
    addResource(newResource) {
      this.storedResources.push({
        id: Date.now(),
        ...newResource,
      });

      this.setSelectedTab('stored-resources');
    },
    deleteResource(id) {
      const resourceIdToDelete = this.storedResources.findIndex(
        (resource) => resource.id === id
      );

      this.storedResources.splice(resourceIdToDelete, 1);
    },
  },
};
</script>
