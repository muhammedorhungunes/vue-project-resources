<template>
  <base-dialog v-if="displayDialogTag" title="Remove Link" @close="rejectDialog">
    <template #default>
      <p>Do you want to remove :</p>
      <p>{{ title }}</p>
    </template>
    <template #actions>
      <base-button @click="rejectDialog">No</base-button>
      <base-button @click="deleteResource(id)">Okay</base-button>
    </template>
  </base-dialog>
  <li>
    <base-card>
      <header>
        <div className="count-card">
          <div className="count">{{ count }}</div>
        </div>
        <h3>{{ title }}</h3>
        <base-button mode="flat" @click="displayDialog()"
          >Delete</base-button
        >
      </header>
      <base-button mode="flat" @click="plusVotedCount(id)">+</base-button>
      <base-button mode="flat" @click="minusVotedCount(id)">-</base-button>
      <p>{{ description }}</p>
      <nav>
        <a :href="link">View Resources</a>
      </nav>
    </base-card>
  </li>
</template>

<script>
export default {
  props: ['id', 'title', 'description', 'link', 'count'],
  inject: ['deleteResource', 'plusVotedCount', 'minusVotedCount'],
  data() {
    return {
      displayDialogTag : false,
    };
  },
  methods: {
    
    displayDialog() {
      this.displayDialogTag = true;
    },
    rejectDialog() {
      this.displayDialogTag = false;
    },
  }
};
</script>

<style>
li {
  margin: auto;
  max-width: 40rem;
}

header {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

h3 {
  font-size: 1.25rem;
  margin: 0.5rem 0;
}

p {
  margin: 0.5rem 0;
}

a {
  text-decoration: none;
  color: #ce5c00;
}

a:hover,
a:active {
  color: #c89300;
}

.count-card {
  display: flex;
  flex-direction: column;
  width: 5.5rem;
  height: 5.5rem;
  border: 1px solid #ececec;
  background-color: #640032;
  color: white;
  border-radius: 12px;
  align-items: center;
  justify-content: center;
}
.count {
  font-size: 1rem;
  font-weight: bold;
}
</style>
