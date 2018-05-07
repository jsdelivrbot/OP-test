<template>
  <div class="container">
    <div class="row">
      <div class="col-md-12 grid">
          <masonry
            :cols="{default: 4, 1000: 3, 700: 2, 400: 1}"
            :gutter="15"
            >
            <div class="header">
                <h1 class="header-title"> Illustrations by
                  <span class="header-name"> Ryo Takemasa </span>
                </h1>
            </div>
          <div v-for="item in items">
            <router-link :to="{ name: 'Post', params: { id: item.id } }">
              <div class="grid-item">
                <img :src="url + item.title"
                      class="grid-item-img"
                      :alt="item.title">
                <span class="grid-item-title">
                  {{ item.title.length > 25 ?
                      item.title.substr(0,25) + '...'
                      : item.title }}
                </span>
              </div>
            </router-link>
          </div>
        </masonry>
      </div>
    </div>
  </div>
</template>

<script>
  import Vue from 'vue'
  import Post from '@/components/Post'
  import VueMasonry from 'vue-masonry-css'

  Vue.use(VueMasonry)

  export default {
    name: 'Home',

    data () {
      return {
        items: [],
        url: "https://source.unsplash.com/random?"
      }
    },
    methods: {
        fetchItems() {
              fetch('https://jsonplaceholder.typicode.com/posts')
                .then(stream => stream.json())
                .then(response => this.items = response)
                .catch(error => console.error(error))
        }
      },
    mounted() {
        this.fetchItems()
    }
  }

</script>

<style lang="scss" scoped>
  @import '@/assets/scss/settings.scss';

    .header {
      display: block;
      position: relative;
      background-color: $c-darken;
      text-align: left;
      color: $c-primary;
      margin: 0;
    }

    .header-title {
      font-weight: 100;
      font-size: 1.5rem;
      text-transform: uppercase;
      padding: 15px;
      margin: 0;
    }

    .header-name {
      display: block;
      color: $c-secondary;
      font-weight: 700;
    }

    .grid {
      margin-top: 50px;
    }

    .grid-item {
      display: block;
      position: relative;
      margin: 10px 0;
      background-color: $c-darken;
      transition: all 0.5s ease;
    }

    .grid-item-title {
      display: block;
      position: absolute;
      width: 100%;
      bottom:0;
      left: 0;
      background-color: $c-darken;
      color: $c-primary;
      text-transform: uppercase;
      font-size: 1.2rem;
      text-align: right;
      padding: 10px;
      transition: all 0.5s ease, color 0.2s ease;
    }

    .grid-item-img {
      display: block;
      position: relative;
      width: 100%;
    }

    .grid-item:hover {
      padding: 10px;
      & > .grid-item-title {
        bottom: 30px;
        color: #fff;
        font-weight: 600;
      }
    }
</style>
