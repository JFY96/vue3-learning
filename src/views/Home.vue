<template>
    <div class="home">
        <h4>Refs</h4>
        <p>My name is {{ person.name }} and I am {{ person.age }} years old</p>
        <input type="text" v-model="person.name">
        <button @click="person.age++">up age</button>
        <button @click="updatePerson">Update age to 40</button>
        <h4>Reactive</h4>
        <p>My name is {{ person2.name }} and I am {{ person2.age }} years old</p>
        <input type="text" v-model="person2.name">
        <button @click="person2.age++">up age</button>
        <button @click="updatePerson2">Update age to 40</button>
        <h4>Computed</h4>
        <input type="text" v-model="search">
        <p>Search: "{{ search }}"</p>
        <div v-for="name in matchingNames" :key="name">{{ name }}</div>
        <h4>watch, watchEffect</h4>
        <button @click="stopWatching">Stop Watching</button>
        <h1>Blog</h1>
        <button @click="showPosts = !showPosts">Toggle Posts</button>
        <button @click="posts.pop()">Delete a Post</button>
        <div v-if="error">{{ error }}</div>
        <div v-if="!error && !posts.length">Loading...</div>
        <PostList v-if="showPosts && posts.length" :posts="posts" />
    </div>
</template>

<script lang="ts">
import { defineComponent, ref, reactive, computed, watch, watchEffect } from 'vue';
import PostList from '../components/PostList.vue';
import getPosts from '../composables/getPosts';

export default defineComponent({
    components: {
        PostList,
    },
    setup () {
        const person = ref({ name: 'Mario', age: 30 });
        const person2 = reactive({ name: 'Mario', age: 30 }); // cannot be primitive value

        const updatePerson = () => {
            person.value.age = 40;
        };

        const updatePerson2 = () => {
            person2.age = 40;
        };

        const search = ref('');
        const names = ref(['Mario', 'Yoshi', 'Luigi', 'Toad', 'Bowser', 'Koopa', 'Peach']);
        const matchingNames = computed(() => {
            return names.value.filter(name => name.toLocaleLowerCase().includes(search.value.toLocaleLowerCase()));
        });

        const stopWatch = watch(search, () => {
            console.log('watch ran since search changed: ', search.value);
        });

        const stopWatchEffect = watchEffect(() => {
            // watches any value used inside this fn
            console.log('watchEffect ran since search.value change (', search.value, ')');
        });

        const stopWatching = () => {
            stopWatch();
            stopWatchEffect();
        };

        const showPosts = ref(true);
        // const posts = ref([
        //     { title: 'Welcome to the blog', body: 'Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.', id: 1 },
        //     { title: 'top 5 tips', body: 'lorem ipsum', id: 2 },
        // ]);
        const { posts, error, load } = getPosts();
        load();

        return { person, person2, updatePerson, updatePerson2, search, matchingNames, stopWatching, posts, showPosts, error };
    }
})
</script>

<style scoped>

</style>