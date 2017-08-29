<template>
    <aside id="master" class="col-xs-5 col-sm-5">
        <div>
            <button class="btn btn-primary" @click="nueva" title="Haga click aquí para crear una entrada">
                <span class="glyphicon glyphicon-plus"></span>
            </button>

        </div>
        <ul class="list-group">
            <li class="list-group-item" v-for="entrada in entradas" :key="entrada.Id" @click="triggerSelect(entrada,$event)" title="Seleccione una entrada para actualizar o borrar">
                {{entrada.Pelicula}}
            </li>
        </ul>
    </aside>
</template>

<script>

export default {
    name: 'master',
    props: ['entradas'],
    data() {
        return {
        };
    },
    methods: {
        nueva: function() {
            this.$emit('nueva');
            $("#master .active").removeClass("active");
        },
        triggerSelect: function(entrada, event) {
            this.$emit('selected', entrada);
            $("#master .active").removeClass("active");
            $(event.target).addClass("active");
        }
    },
    updated() {
        $("#master .active").removeClass("active");
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

li.active{
    /* font-weight: bold; */
    background-color: #286090;
}
</style>
