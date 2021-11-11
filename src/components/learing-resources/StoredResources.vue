<template>
    <ul>
      <learing-resource
        v-for="res in resources"
        :key="res.id"
        :id="res.id"
        :title="res.title"
        :description="res.descriptions"
        :link="res.link"
        :count="res.count"
      ></learing-resource>
    </ul>
</template>

<script>
import LearingResource from './LearingResource.vue';

export default {
  inject: ['resources'],
  components: {
    LearingResource,
  },
};
</script>

<style scoped>
ul {
  list-style: none;
  margin: 0;
  padding: 0;
  margin: auto;
  max-width: 40rem;
}
</style>
