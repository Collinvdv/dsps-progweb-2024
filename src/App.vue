<template>
  <div id="app">
    <!-- Navigation -->
    <nav>
      <ul>
        <li 
          v-for="page in pages" 
          :key="page"
          @click="setActivePage(page)"
          :style="getActiveState(page)">
          {{ page }}
        </li>
      </ul>
    </nav>

    <!-- Actual page -->
    <h1>
      {{ activePage }}
    </h1>

    <PageHome v-if="activePage == pages[0]"/>
    <PageRanking v-if="activePage == pages[1]"/>
    <PageVoting v-if="activePage == pages[2]"/>
  </div>
</template>

<script>
  import PageHome from './pages/PageHome.vue';
  import PageVoting from './pages/PageVoting.vue';
  import PageRanking from './pages/PageRanking.vue';

  export default {
    name: 'App',
    components: {
      PageHome,
      PageRanking,
      PageVoting
    },
    data() {
      return {
        pages: [
          "home",
          "ranking",
          "voting"
        ],
        activePage: "ranking"
      }
    },
    methods: {
      setActivePage(page) {
        this.activePage = page;
      },
      getActiveState(page) {
        if (this.activePage == page) {
          return "text-decoration: underline";
        }
      }
    }
  }
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
