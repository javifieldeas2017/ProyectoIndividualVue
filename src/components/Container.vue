<template>
    <section id="container">
        <keep-alive>
            <component :is="this.selectedSection"></component>
        </keep-alive>
    </section>
</template>

<script>
import entradas from './Entradas/Entradas.vue'
import peliculas from './Peliculas/Peliculas.vue'
export default {
    name: 'container',
    props: ['selectedSection'],
    data() {
        return {
        };
    },
    components: {
        entradas,peliculas
    }
}
</script>

<style scoped>
</style>
