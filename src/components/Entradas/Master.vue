<template>
    <aside id="master" class="col-xs-5 col-sm-5">
        <div>
            <button class="btn btn-primary" @click="nueva" title="Haga click aquí para crear una entrada">
                <span class="glyphicon glyphicon-plus"></span>
            </button>
            <h1>Entradas</h1>
        </div>
        <ul class="list-group">
            <li class="list-group-item" v-for="entrada in entradas" :key="entrada.Id" @click="triggerSelect(entrada,$event) + setActive(entrada)" :class="{ active: isActive(entrada)}" title="Seleccione una entrada para actualizar o borrar">
                {{entrada.Pelicula}}
            </li>
        </ul>
    </aside>
</template>

<script>

export default {
    name: 'master',
    props: ['entradas','selected'],
    data() {
        return {
        };
    },
    methods: {
        nueva: function() {
            this.$emit('nueva');
            var vacio = {}; vacio.Id = "";
            this.setActive(vacio);
        },
        triggerSelect: function(entrada, event) {
            this.$emit('selected', entrada);
        },
        isActive: function(entrada) {
            return this.selected.Id == entrada.Id
        },
        setActive: function(entrada) {
            this.selected.Id = entrada.Id
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
