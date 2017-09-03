<template>
  <div class="feed">

    <div class="form-group">

      <select v-model="current" class="form-control" @change="getPosts()">
        <option v-for="feed in feeds" :value="feed.url">{{ feed.label }}</option>
      </select>
    </div>

    <div v-for="item in items" class="row">
      <a :href="item.link" target="_blank" class="reset_link">
        <div class="card" v-if="item.enclosure.link">
          <img style="width:100%" :src="item.enclosure.link" :alt="item.title" />
          <div class="card-body">
            <h4 class="card-tittle">{{ item.title }}</h4>
            <p class="card-text">
              {{ getDescription(item.description) }}
            </p>
            <a target="_blank" :href="item.link" class="btn btn-primary">Ver más</a>
          </div>
        </div>
      </a>
    </div>
  </div>
</template>

<script type="text/ecmascript-6">


/* eslint-disable arrow-parens, func-names, space-before-function-paren, no-shadow, padded-blocks,object-shorthand, comma-dangle, max-len   */
import axios from 'axios';

const feed = (url) => `https://api.rss2json.com/v1/api.json?rss_url=${url}`;

export default {
  name: 'feed',
  data() {
    return {
      current: 'http://www.eluniverso.com/rss/politica.xml',
      feeds: [{
        label: 'Politica',
        url: 'http://www.eluniverso.com/rss/politica.xml',
      }, {
        label: 'Economia',
        url: 'http://www.eluniverso.com/rss/economia.xml',
      }, {
        label: 'Futbol',
        url: 'http://www.eluniverso.com/rss/futbol.xml'
      }],
      items: [],
    };
  },
  mounted() {
    this.getPosts();
  },

  methods: {
    getPosts() {

      axios.get(feed(this.current))
        .then((response) => {
          if (response.status === 200) {
            const items = response.data.items;
            this.items = items;
          }
        });
    },
    getDescription(description) {
      const resumen = description.substr(0, 140);
      const palabras = resumen.split(' ');
      const nuevoParrafo = palabras.splice(0, palabras.length - 1);
      return nuevoParrafo.join(' ');
    }
  },
};
</script>

<style>
html body {
  overflow-x: hidden;
}

body {
  width: 100%;
  margin: 0;
}

.feed {
  width: 90%;
  display: inline-block;
}

.card {
  margin-bottom: 1rem;
}

.reset_link,
.reset_link:hover,
.reset_link:visited {
  text-decoration: none;
  color: black;
}
</style>
