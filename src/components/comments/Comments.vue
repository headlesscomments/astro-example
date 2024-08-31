<template>
  <div>
    <h2>Comments</h2>

    <!-- Comments List -->
    <ul v-if="isLoaded">
      <li v-for="comment in comments" :key="comment.id">
        <span>{{ comment.content }}</span>
        <div>
          <strong>{{ comment.author_name }}</strong> -
          <span>{{ new Date(comment.created_at).toLocaleDateString() }}</span>
        </div>
      </li>
    </ul>

    <!-- Loading Spinner -->
    <div v-else>Loading...</div>

    <!-- Message Box -->
    <div class="message-box">
      <form @submit.prevent="submit">
        <div class="author-info">
          <input type="text" placeholder="Name" v-model="comment.author_name" required />
          <input type="email" placeholder="Email" v-model="comment.author_email" required />
        </div>
        <textarea rows="4" placeholder="Write your comment here..." v-model="comment.content" required></textarea>
        <div class="button-container">
          <button>Submit</button>
        </div>
      </form>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, onMounted } from "vue";

import api from "../../comments";
import type { Page, Comment } from "@headlesscomments/sdk/dist/types";

const props = defineProps<{
  path: string;
}>();

const isLoaded = ref(false);
const isSending = ref(false);

const page = ref({} as Page);
const comments = ref([] as Comment[]);

const comment = ref({
  author_name: "",
  author_email: "",
  content: "",
});

async function fetchComments() {
  isLoaded.value = false;

  page.value = await api.pages.get(props.path);

  if (page.value && page.value.allow_comments) {
    comments.value = (await api.comments.list(page.value.id)).data;
  }

  isLoaded.value = true;
}

async function submit() {
  isSending.value = true;
  const response = await api.comments.create(props.path, comment.value);

  if (response.message === "Comment ingested successfully.") {
    alert("Comment submitted successfully!");
  } else {
    alert("Failed to submit comment. Please try again.");
  }

  // Clear the form
  comment.value = {
    author_name: "",
    author_email: "",
    content: "",
  };

  await fetchComments();
  isSending.value = false;
}

onMounted(async () => {
  await fetchComments();
});
</script>

<style scoped>
ul {
  list-style: none;
  padding: 0;
}

li {
  margin-bottom: 1rem;
  padding: 1rem;
  border: 1px solid #ccc;
}

.message-box {
  margin-top: 1rem;
  border: 1px solid #ccc;
  padding: 1rem;
}

.message-box .author-info {
  display: grid;
  gap: 0.5rem;
}

.author-info input {
  margin-bottom: 0.5rem;
  border-radius: 4px;
  border: 1px solid #ccc;
  padding: 0.5rem;
}

.button-container {
  display: flex;
  justify-content: flex-end;
}

.message-box button {
  padding: 0.5rem 1rem;
  background-color: #007bff;
  color: white;
  border: none;
  cursor: pointer;
  border-radius: 4px;
}
</style>
