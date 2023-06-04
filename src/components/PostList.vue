<template>
    <div class="post-list">
        <div v-for="post in posts" :key="post.id">
            <SinglePost :post="post" />
        </div>
    </div>
</template>

<script lang="ts">
import { defineComponent, onMounted, onUnmounted, onUpdated } from 'vue';
import SinglePost from './SinglePost.vue';

export default defineComponent({
    components: {
        SinglePost,
    },
    props: ['posts'],
    setup (props) {
        onMounted(() => {
            console.log('onMounted');
        });
        onUnmounted(() => {
            console.log('onUnmounted');
        });
        onUpdated(() => {
            console.log('onUpdated');
        });

        return {};
    },

})
</script>

<style scoped>

</style>