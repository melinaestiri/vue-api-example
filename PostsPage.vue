<template>
  <div class="container">
    <h2>Posts</h2>

    <div v-if="loading" class="status">
      Loading...
    </div>

    <div v-else-if="error" class="status error">
      {{ error }}
    </div>

    <ul v-else class="posts">
      <li v-for="post in posts" :key="post.id">
        <h4>{{ post.title }}</h4>
        <p>{{ post.body }}</p>
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  name: "PostsPage",
  data() {
    return {
      posts: [],
      loading: false,
      error: null
    };
  },
  mounted() {
    this.fetchPosts();
  },
  methods: {
    async fetchPosts() {
      this.loading = true;
      this.error = null;

      try {
        const response = await fetch(
          "https://jsonplaceholder.typicode.com/posts?_limit=5"
        );

        if (!response.ok) {
          throw new Error("Failed to load data");
        }

        this.posts = await response.json();
      } catch (err) {
        this.error = "An error occurred while fetching data.";
      } finally {
        this.loading = false;
      }
    }
  }
};
</script>

<style>
.container {
  max-width: 600px;
  margin: 40px auto;
  font-family: sans-serif;
}

h2 {
  margin-bottom: 16px;
}

.status {
  padding: 12px;
  background: #f0f0f0;
  border-radius: 6px;
}

.status.error {
  background: #ffe0e0;
  color: #900;
}

.posts {
  list-style: none;
  padding: 0;
}

.posts li {
  border: 1px solid #ddd;
  border-radius: 6px;
  padding: 12px;
  margin-bottom: 12px;
}

.posts h4 {
  margin: 0 0 6px;
  font-size: 15px;
}

.posts p {
  margin: 0;
  font-size: 14px;
  color: #555;
}
</style>
