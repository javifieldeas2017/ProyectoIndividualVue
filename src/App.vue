<template>
  <div id="app">
    <navigator @sectionClicked="changeSection"></navigator>
    <container :selectedSection="currentSection"></container>
  </div>
</template>

<script>
import navigator from './components/Navigator.vue'
import container from './components/Container.vue'

export default {
  name: 'app',
  data() {
    return {
      currentSection: null
    }
  },
  components: {
    navigator, container
  },
  methods: {
    changeSection: function(sectionNombre) {
      this.currentSection = sectionNombre;
    }
  }
}
</script>

<style>
* {
  font-family: Montserrat;
}

li:hover {
  cursor: pointer;
}
</style>
