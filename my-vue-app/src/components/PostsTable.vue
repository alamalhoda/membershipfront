<template>

  <v-app>
    <v-main>
      <v-container>...!!!???</v-container>

      <v-container>
        <v-data-table
            :headers="headers"
            :items="posts"
            :items-per-page="5"
            class="elevation-1"
        ></v-data-table>
      </v-container>
    </v-main>
  </v-app>
</template>

<script>
import { ref, onMounted } from 'vue';
import axios from 'axios';

export default {
  name: 'PostsTable',
  setup() {
    const posts = ref([]);
    const headers = [
      { text: 'ID', value: 'id' },
      { text: 'Title', value: 'title' },
      { text: 'Body', value: 'body' }
    ];

    const fetchPosts = async () => {
      try {
        const response = await axios.get('https://jsonplaceholder.typicode.com/posts');
        posts.value = response.data;
      } catch (error) {
        console.error('Error fetching posts:', error);
      }
    };

    onMounted(() => {
      fetchPosts();
    });

    return {
      posts,
      headers
    };
  }
};
</script>

<style scoped>
/* شما می‌توانید استایل‌های اضافی خود را اینجا اضافه کنید */
</style>