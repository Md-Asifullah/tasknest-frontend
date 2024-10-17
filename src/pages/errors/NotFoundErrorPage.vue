<template>
    <div class="container py-5">
        <h1>404</h1>
        <p>
            Oops! The page
            <b
                ><code>{{ $route.params.notFound }}</code></b
            >
            you followed leads to a nowhere zone. Let’s not linger here.
        </p>
        <router-link :to="{ name: 'tasks' }">Back to Tasks Page</router-link>
    </div>
</template>

<style scoped>
code {
    color: #198754;
}
</style>
