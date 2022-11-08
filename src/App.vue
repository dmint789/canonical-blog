<script setup lang="ts">
import { ref } from 'vue';
import axios from 'axios';

const posts = ref<Array<any>>([]);

const getPosts = async () => {
  axios.get('https://people.canonical.com/~anthonydillon/wp-json/wp/v2/posts.json').then((res) => {
    posts.value = res.data;
    console.log(posts.value);
  });
};
getPosts();

const getPrettyDate = (date: string): string => {
  return new Date(Date.parse(date))
    .toLocaleDateString('default', { day: 'numeric', month: 'long', year: 'numeric' })
    .toString();
};
</script>

<template>
  <div>
    <div class="container">
      <div class="row">
        <div v-for="post in posts" class="col-4 bottom-margin">
          <div class="post">
            <div>
              <!-- Topic -->
              <p class="p-text--x-small-capitalised">
                {{ post._embedded['wp:term'][2][0]?.name || 'ubuntu' }}
              </p>
              <hr />
              <a :href="post.link"><img :src="post.featured_media" alt="Post image" /></a>
              <!-- Title -->
              <h4>
                <a :href="post.link">{{ post.title.rendered }}</a>
              </h4>
              <!-- Author and date -->
              <p>
                <i>
                  By&#32;<a :href="post._embedded.author[0].link" target="_blank">
                    {{ post._embedded.author[0].name }}</a
                  >&#32;on&#32;{{ getPrettyDate(post.date) }}
                </i>
              </p>
            </div>
            <div>
              <hr />
              <p class="lastP">Article</p>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<style lang="scss" scoped>
@import './assets/main.scss';

.container {
  max-width: 1000px;
  margin: $spv--strip-regular auto;
}

.bottom-margin {
  margin-bottom: $sp-medium;
}

.post {
  height: 100%;
  padding: $sp-medium;
  border-top: 3px solid #a87ca0;
  border-radius: 3px;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  box-shadow: 0 0 5px rgba(0, 0, 0, 0.3);

  .capitalized {
    text-transform: uppercase;
  }

  .lastP {
    margin-bottom: 0;
  }
}
</style>
