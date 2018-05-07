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
          <div v-for="(item, index) in items">
            <div :key="index" @click="zoomItem(index)" v-bind:class="currentItem == index ? 'active' : ''">
              <div  v-bind:class="currentItem == index ? 'grid-item grid-item--active' : 'grid-item'">
                <img :src="imageUrl + item.title"
                      class="grid-item-img"
                      :alt="item.title">
                <span v-if="currentItem != index" class="grid-item-title">
                  {{ item.title.length > 25 ?
                      item.title.substr(0,25) + '...'
                      : item.title }}
                </span>
                <span v-else class="grid-item-title grid-item-title--active">
                    {{ item.title }}
                    <span class="grid-item-body">
                        {{ item.body }}
                     </span>
                </span>
              </div>
            </div>
          </div>
        </masonry>
      </div>
    </div>
  </div>
</template>

<script>

  import Vue from 'vue'
  import VueMasonry from 'vue-masonry-css'

  Vue.use(VueMasonry)

  export default {
    name: 'Home',

    data () {
      return {
        open: false,
        items: [],
        imageUrl: "https://source.unsplash.com/random?",
        currentItem: null
        }
    },
    methods: {
        fetchItems() {
              fetch('https://jsonplaceholder.typicode.com/posts')
                .then(stream => stream.json())
                .then(response => this.items = response)
                .catch(error => console.error(error))
        },
        zoomItem(index) {
            this.currentItem === index ? this.currentItem = null : this.currentItem = index
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
      margin: 10px;
    }

    .header-name {
      display: block;
      color: $c-secondary;
      font-weight: 700;
    }

    .grid {
      margin-top: 20px;
    }

    .grid-item {
      display: block;
      position: relative;
      margin: 10px 0;
      background-color: $c-darken;
      transition: all 0.5s ease;
      cursor: pointer;
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

    .grid-item:not(.grid-item--active):hover {
      padding: 10px;
      & > .grid-item-title {
        bottom: 30px;
        color: #fff;
        font-weight: 600;
      }
    }

    .hidden {
        display: none;
    }

    .active {
       position: fixed;
       width: 100%;
       height: 100vh;
       background-color: rgba(0,0,0,0.7);
       top: 0;
       left: 0;
       z-index: 100;
    }

    .grid-item--active {
       position: relative;
       width: auto;
       margin-top: 40px;
       .grid-item-img {
            max-height: 80vh;
            width: auto;
            max-width: 100%;
            margin: auto;
       }
    }

    .grid-item-title--active {
        position: absolute;
        bottom: 0;
        font-weight: 900;
    }

    .grid-item-body {
        display: block;
        border-top: 1px solid rgba(255,255,255,0.1);
        padding-top: 10px;
        font-weight: 100;
        font-size: 1rem;
        text-transform: none;
    }

</style>
