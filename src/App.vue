<template>
  <div id="app">
    <header class="title">{{ title }}</header>
    <Repolist />
    <Container />
    <div ref="listElm" class="more-content" />
    <Loading />
  </div>
</template>

<script>
import Repolist from './components/repolist.vue'
import Container from './components/container.vue'
import Loading from './components/loading.vue'

const { Octokit } = require("@octokit/core");
const octokit = new Octokit();

export default {
  name: 'App',
  components: {
    Repolist,
    Container,
    Loading
  },
  data() {
    return {
      title: 'All The Repos',
      contents: []
    }
  },
  mounted() {
    window.addEventListener('scroll', () => {
      const {scrollHeight, scrollTop, clientHeight} = document.documentElement
      if(scrollTop + clientHeight >= scrollHeight - 10) {
        setTimeout(this.loadMore, 1500)
      }
    })
  },
  methods: {
    async fetchData() {
      let response = await octokit.request('GET /repositories')
      this.contents = response.data
    },
    loadMore() {
      this.fetchData()
      for(let content of this.contents) {
        let item = document.createElement('div');
        item.className = "list-item";
        item.innerHTML = `<h2>${content.name}</h2>
          <p>${content.description}</p>
          <p class="link">${content.html_url}</p>`;
        this.$refs.listElm.appendChild(item)
      }
    }
  }
}
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Quicksand:wght@400;600&display=swap');

html {
  background-color: rgba(245, 219, 164, .1);
}

html body #app {
  height: 100%;
}
#app {
  font-family: 'Quicksand', sans-serif;
}
.title {
  font-size: 5rem;
  font-weight: 600;
  display: flex;
  justify-content: center;
  align-items: center;
  height: 200px;
}
.more-content {
  width: 90%;
  margin: 2.5rem auto 0 auto;
  display: flex;
  flex-wrap: wrap;
}
.list-item {
  box-sizing: border-box;
  width: 25%;
  padding: 2%;
  word-break: break-word;
}
.list-item:hover {
  background-color: rgba(245, 219, 164, .7);
  border-radius: 2px;
  cursor: pointer;
}
.link {
  font-size: .8rem;
}
</style>
