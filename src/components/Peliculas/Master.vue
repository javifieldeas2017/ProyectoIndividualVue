<template>
    <aside id="master" class="col-xs-5 col-sm-5">
        <div>
            <button class="btn btn-primary" @click="nueva" title="Haga click aquí para crear una pelicula">
                <span class="glyphicon glyphicon-plus"></span>
            </button>
            <h1>Películas</h1>
        </div>
        <ul class="list-group">
            <li class="list-group-item" v-for="pelicula in peliculas" :key="pelicula.Id" @click="triggerSelect(pelicula,$event) + setActive(pelicula)" :class="{ active: isActive(pelicula)}" title="Seleccione una pelicula para actualizar o borrar">
                {{pelicula.Titulo}}
            </li>
        </ul>
    </aside>
</template>

<script>

export default {
    name: 'master',
    props: ['peliculas','selected'],
    data() {
        return {
        };
    },
    methods: {
        nueva: function() {
            this.$emit('nueva');
            var vacio = {}; vacio.Id="";
            this.setActive(vacio);
        },
        triggerSelect: function(pelicula, event) {
            this.$emit('selected', pelicula);
        },
        isActive: function(pelicula) {
            return this.selected.Id == pelicula.Id
        },
        setActive: function(pelicula) {
            this.selected.Id = pelicula.Id // no need for Vue.set()
        }
    }
}

</script>

<style scoped>
button {
    margin-bottom: 20px;
}





/* li:hover {
    background-color: #337ab7;
    border-color: #337ab7;
    color:#FFF;
} */

h1 {
    display: inline-block;
    margin: 0;
    padding-left: 30px;
    text-align: center;
}

li.active {
    /* font-weight: bold; */
    background-color: #286090;
}
</style>
