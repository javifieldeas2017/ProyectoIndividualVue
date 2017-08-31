<template>
    <div id="peliculas">
        <master :peliculas="peliculas" :selected="selected" @nueva="unsetSelected" @selected="setSelected"></master>
        <detail :newMode="newMode" :selected="selected" @callAddPelicula="addPelicula" @callUpdatePelicula="updatePelicula" @callDeletePelicula="deletePelicula"></detail>
    </div>
</template>

<script>
import master from './Master.vue'
import detail from './Detail.vue'
export default {
    name: 'peliculas',
    data() {
        return {
            peliculas: [],
            newMode: true,
            selected: null
        };
    },
    components: {
        master, detail
    },
    methods: {
        getPeliculas: function() {
            let _this = this;
            $.ajax({
                type: 'GET',
                url: 'http://localhost:51398/api/Peliculas/',
                success: function(response) {
                    _this.peliculas = response;
                }
            });
        },
        addPelicula: function(pelicula) {
            let _this = this;
            $.ajax({
                type: 'POST',
                url: 'http://localhost:51398/api/Peliculas/',
                data: pelicula,
                success: function(response) {
                    _this.peliculas.push(response);
                    _this.unsetSelected();
                },
                error: this.error
            });
        },
        updatePelicula: function(pelicula) {
            let _this = this;
            $.ajax({
                type: 'PUT',
                url: 'http://localhost:51398/api/Peliculas/' + pelicula.Id,
                data: pelicula,
                success: function(response) {
                    var index = _this.peliculas.findIndex((el) => el.Id == pelicula.Id);
                    _this.peliculas[index] = pelicula;
                    _this.peliculas = JSON.parse(JSON.stringify(_this.peliculas))
                    _this.unsetSelected();
                },
                error: this.error
            });
        },
        deletePelicula: function(pelicula) {
            let _this = this;
            $.ajax({
                type: 'DELETE',
                url: 'http://localhost:51398/api/Peliculas/' + pelicula.Id,
                success: function(response) {
                    var index = _this.peliculas.findIndex((el) => el.Id == pelicula.Id);
                    _this.peliculas.splice(index, 1);
                    _this.selected = null;
                    _this.unsetSelected();
                },
                error: this.error
            });
        },
        error: function(xhr, textStatus, errorThrown) {
            alert("Error!->" + errorThrown + "-->" + xhr.responseText);
        },
        unsetSelected: function() {
            this.newMode = true;
            this.selected = { Titulo: "", Pais: "", Duracion: "", Genero: "", Director:"", Sinopsis:"" }
        },
        setSelected: function(pelicula) {
            this.newMode = false;
            this.selected = JSON.parse(JSON.stringify(pelicula));
        }

    },
    created() {
        this.getPeliculas();
        this.unsetSelected();
    }
}

</script>

<style scoped>

</style>
