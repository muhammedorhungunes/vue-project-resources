<template>
  <base-card>
    <base-button
      @click="setSelectedTab('stored-resources')"
      :mode="storedResButtonMode"
      >Stored Resources</base-button
    >
    <base-button
      @click="setSelectedTab('add-resource')"
      :mode="addResButtonMode"
      >Add Resources</base-button
    >
  </base-card>

  <add-resource v-if="selectedTab === 'add-resource'"></add-resource>
  <div>
    <select v-model="sortedTrigger" v-if="selectedTab == 'stored-resources'">
      <option value="Pagination">Pagination</option>
      <option value="Most">Most</option>
      <option value="Less">Less</option>
    </select>
  </div>
  <ul
    v-if="
      selectedTab == 'stored-resources' &&
      sortedTrigger !== 'Most' &&
      sortedTrigger !== 'Less' &&
      sortedTrigger === 'Pagination'
    "
  >
    <learing-resource
      v-for="res in paginatedData"
      :key="res.id"
      :id="res.id"
      :title="res.title"
      :description="res.descriptions"
      :link="res.link"
      :count="res.count"
    ></learing-resource>
  </ul>
  <ul v-if="selectedTab == 'stored-resources' && sortedTrigger === 'Most'">
    <learing-resource
      v-for="res in sortedMostArray"
      :key="res.id"
      :id="res.id"
      :title="res.title"
      :description="res.descriptions"
      :link="res.link"
      :count="res.count"
    ></learing-resource>
  </ul>
  <ul v-if="selectedTab == 'stored-resources' && sortedTrigger === 'Less'">
    <learing-resource
      v-for="res in sortedLessArray"
      :key="res.id"
      :id="res.id"
      :title="res.title"
      :description="res.descriptions"
      :link="res.link"
      :count="res.count"
    ></learing-resource>
  </ul>
  <base-button
    v-if="selectedTab == 'stored-resources'"
    @click="prevPage"
    :disabled="pageNumber == 0"
  >
    Previous
  </base-button>
  <base-button
    v-if="selectedTab == 'stored-resources'"
    @click="nextPage"
    :disabled="pageNumber >= pageCount - 1"
  >
    Next
  </base-button>
</template>

<script>
import BaseButton from '../UI/BaseButton.vue';
import AddResource from './AddResource.vue';
// import StoredResources from './StoredResources.vue';
// import AddResource from './AddResource.vue';
import LearingResource from './LearingResource.vue';

export default {
  components: {
    // StoredResources,
    // AddResource,
    LearingResource,
    AddResource,
    BaseButton,
  },
  data() {
    return {
      selectedTab: 'stored-resources',
      storedResources: [
        {
          id: 'official-guide',
          title: 'Official Guide ',
          descriptions: 'The Official Vue.js documentation.',
          link: 'https://vuejs.org',
          count: 0,
        },
      ],
      pageNumber: 0,
      size: 2,
      sortedTrigger: '',
    };
  },

  provide() {
    return {
      resources: this.storedResources,
      addResource: this.addResource,
      deleteResource: this.removeResource,
      plusVotedCount: this.plusVotedCount,
      minusVotedCount: this.minusVotedCount,
    };
  },

  computed: {
    storedResButtonMode() {
      return this.selectedTab === 'stored-resources' ? null : 'flat';
    },
    addResButtonMode() {
      return this.selectedTab === 'add-resource' ? null : 'flat';
    },
    pageCount() {
      let l = this.storedResources.length,
        s = this.size;
      return Math.ceil(l / s);
    },
    paginatedData() {
      const start = this.pageNumber * this.size,
        end = start + this.size;
      return this.storedResources.slice(start, end);
    },
    sortedMostArray: function () {
      function compare(a, b) {
        if (a.count > b.count) return -1;
        if (a.count < b.count) return 1;
        return 0;
      }
      const start = this.pageNumber * this.size,
        end = start + this.size;
      return this.storedResources.slice().sort(compare).slice(start, end);
    },
    sortedLessArray: function () {
      function compare(a, b) {
        if (a.count < b.count) return -1;
        if (a.count > b.count) return 1;
        return 0;
      }
      const start = this.pageNumber * this.size,
        end = start + this.size;
      return this.storedResources.slice().sort(compare).slice(start, end);
    },
  },
  methods: {
    nextPage() {
      this.pageNumber++;
    },
    prevPage() {
      this.pageNumber--;
    },
    setSelectedTab(tab) {
      this.selectedTab = tab;
    },
    addResource() {
      const newResource = {
        id: new Date().toISOString(),
        title: localStorage.getItem('enteredTitle'),
        descriptions: localStorage.getItem('enteredDescription'),
        link: localStorage.getItem('enteredLink'),
        count: 0,
      };
      this.storedResources.unshift(newResource);
      this.selectedTab = 'stored-resources';
      this.nextId++;
    },
    removeResource(resId) {
      const resIndex = this.storedResources.findIndex(
        (res) => res.id === resId
      );
      this.storedResources.splice(resIndex, 1);
      this.nextId--;
    },
    plusVotedCount(votId) {
      const votIndex = this.storedResources.findIndex(
        (res) => res.id === votId
      );
      this.storedResources[votIndex].count++;
    },
    minusVotedCount(votId) {
      const votIndex = this.storedResources.findIndex(
        (res) => res.id === votId
      );
      if (this.storedResources[votIndex].count !== 0)
        this.storedResources[votIndex].count--;
    },
  },
};
</script>
<style  scoped>
select {
  display: block;
  margin: 0 auto;
  width: 5.5em
}
</style>
