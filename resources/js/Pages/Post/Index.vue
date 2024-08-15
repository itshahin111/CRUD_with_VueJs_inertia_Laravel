<template>
  <div class="container">
    <h1 class="title">My Inertia CRUD</h1>
    <Link href="/posts/create" class="btn btn-primary">Create new Post</Link>
    <table class="table">
      <thead>
        <tr>
          <th v-for="header in headers" :key="header">{{ header }}</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="post in posts" :key="post.id">
          <td>{{ post.title }}</td>
          <td>{{ post.body }}</td>
          <td class="actions">
            <!-- Delete button with confirmation -->
            <button @click="deletePost(post.id)" class="btn btn-danger">Delete</button>
            <!-- Edit link -->
            <Link :href="`/posts/${post.id}/edit`" class="btn btn-warning">Edit</Link>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script setup>
import { Link, useForm } from '@inertiajs/vue3';
import { ref } from 'vue';

// Define props
const props = defineProps({
  posts: {
    type: Array,
    default: () => [],
  },
});

// Table headers
const headers = ['Title', 'Body', 'Actions'];

// Initialize form for handling deletion
const form = useForm({});

// Function to handle post deletion
const deletePost = (id) => {
  if (confirm('Are you sure you want to delete this post?')) {
    form.delete(`/posts/${id}`);
  }
};
</script>

<style scoped>
/* Container styling */
.container {
  max-width: 1200px;
  margin: 0 auto;
  padding: 20px;
}

/* Title styling */
.title {
  font-size: 2em;
  margin-bottom: 20px;
  color: #333;
}

/* Button styling */
.btn {
  display: inline-block;
  padding: 0.5em 1em;
  font-size: 1em;
  font-weight: 600;
  color: #fff;
  border: none;
  border-radius: 0.25em;
  text-align: center;
  cursor: pointer;
  text-decoration: none;
  margin-right: 10px;
}

.btn-primary {
  background-color: #007bff;
}

.btn-primary:hover {
  background-color: #0056b3;
}

.btn-danger {
  background-color: #dc3545;
}

.btn-danger:hover {
  background-color: #c82333;
}

.btn-warning {
  background-color: #ffc107;
  color: #212529;
}

.btn-warning:hover {
  background-color: #e0a800;
}

/* Table styling */
.table {
  width: 100%;
  border-collapse: collapse;
  margin-top: 20px;
}

thead th {
  background-color: #f4f4f4;
  padding: 10px;
  text-align: left;
  font-weight: bold;
}

tbody td {
  border: 1px solid #ddd;
  padding: 10px;
}

.actions {
  text-align: center;
}
</style>
