<template>
  <ViewContainer :title="project_key">
    <div class="data-container">
      <div class="data-column">
        <div class="subtitle">Versions:</div>
        <Version v-for="version in project.versions" :version="version" :key="version.code"/>
        <PlusButton @click="newVersion"/>
      </div>
    </div>
    <div class="column-container">
      <Column v-for="(column, index) in columns"
              :column="column"
              :columnIndex="index"
              :key="column.key"/>
    </div>
  </ViewContainer>
</template>

<script>
import ViewContainer from '@/components/ViewContainer.vue'
import Column from '@/components/project-elements/Column.vue'
import Version from '@/components/project-elements/Version.vue'
import PlusButton from '@/components/buttons/PlusButton.vue'

export default {
  name: 'ProjectShowPage',
  components: {
    ViewContainer,
    Column,
    Version,
    PlusButton
  },
  computed: {
    project() {
      return this.$store.state.project
    },
    columns() {
      return this.$store.state.visibleTasks
    },
    project_key() {
      return this.$store.state.currentProject
    }
  },
  methods: {
    newVersion: function() {
      this.$store.dispatch('newVersion')
    }
  },
  mounted: function() {
    this.$store.dispatch('setCurrentProject', this.$route.params.id)
    this.$store.dispatch('fetchProject')
    this.$store.dispatch('fetchTasks')
  },
}
</script>

<style scoped lang="scss">
@import "../assets/sass/variables";

.data-container {
  display: flex;
  justify-content: center;
  flex-direction: row;
  padding: 10px;

  .data-column {
    display: flex;
    justify-content: center;
    align-items: center;
    flex-direction: row;
    text-align: center;
    flex-wrap: wrap;

    .subtitle {
      font-weight: 550;
    }
  }
}

.column-container {
  display: flex;
  justify-content: center;
  flex-direction: row;
  flex-wrap: wrap;
}
</style>
