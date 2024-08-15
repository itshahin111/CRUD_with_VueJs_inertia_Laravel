<template>
    <div class="form-container">
        <h1 class="form-title">{{ isUpdating ? 'Update Post' : 'Create Post' }}</h1>
        <form @submit.prevent="submit" class="post-form">
            <div class="form-group">
                <label for="title">Title</label>
                <input type="text" id="title" v-model="form.title" placeholder="Enter title" />
                <span v-if="form.errors.title" class="error-message">{{ form.errors.title }}</span>
            </div>
            <div class="form-group">
                <label for="body">Body</label>
                <textarea id="body" v-model="form.body" placeholder="Enter body"></textarea>
                <span v-if="form.errors.body" class="error-message">{{ form.errors.body }}</span>
            </div>
            <button type="submit" class="btn btn-primary">Submit</button>
        </form>
    </div>
</template>

<script setup>
import { ref, watch } from 'vue';
import { useForm } from '@inertiajs/vue3';

// Define props
const props = defineProps({
    post: {
        type: Object,
        default: () => ({}), // Default to an empty object
    },
    isUpdating: {
        type: Boolean,
        default: false,
    },
});

// Initialize form with useForm
const form = useForm({
    title: "",
    body: "",
});

// Watch for changes to props.post and update the form if isUpdating
watch(() => props.post, (newPost) => {
    if (props.isUpdating) {
        form.title = newPost.title;
        form.body = newPost.body;
    }
}, { immediate: true });

const submit = () => {
    if (props.isUpdating) {
        updatePost();
    } else {
        addPost();
    }
};

// Add a new post
const addPost = () => {
    form.post("/posts", {
        onError: (errors) => {
            // Handle errors here if needed
            console.log(errors);
        },
    });
};

// Update an existing post
const updatePost = () => {
    form.put(`/posts/${props.post.id}`, {
        onError: (errors) => {
            // Handle errors here if needed
            console.log(errors);
        },
    });
};
</script>

<style scoped>
/* Container styling */
.form-container {
    max-width: 600px;
    margin: 0 auto;
    padding: 20px;
    background-color: #f9f9f9;
    border-radius: 8px;
    box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
}

/* Form title styling */
.form-title {
    font-size: 1.5em;
    margin-bottom: 20px;
    color: #333;
    text-align: center;
}

/* Form styling */
.post-form {
    display: flex;
    flex-direction: column;
}

.form-group {
    margin-bottom: 15px;
}

.form-group label {
    display: block;
    margin-bottom: 5px;
    font-weight: bold;
}

.form-group input, 
.form-group textarea {
    width: 100%;
    padding: 10px;
    border: 1px solid #ccc;
    border-radius: 4px;
}

.form-group textarea {
    height: 100px;
    resize: vertical;
}

/* Error message styling */
.error-message {
    color: #dc3545;
    font-size: 0.875em;
    margin-top: 5px;
}

/* Button styling */
.btn {
    display: inline-block;
    padding: 10px 20px;
    font-size: 1em;
    font-weight: 600;
    color: #fff;
    border: none;
    border-radius: 4px;
    text-align: center;
    cursor: pointer;
    text-decoration: none;
}

.btn-primary {
    background-color: #007bff;
}

.btn-primary:hover {
    background-color: #0056b3;
}
</style>
