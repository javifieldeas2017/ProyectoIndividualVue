<template>
    <section id="detail" class="col-xs-7 col-sm-7">

        <div class="form-group">
            <label for="titulo">Título</label>
            <input name="titulo" type="text" v-model="selected.Titulo" class="form-control input-sm" placeholder="Introduce película" @input="validatePelicula()" @keyup="validatePelicula()" title="Escribe un título de máximo 20 caracteres">
        </div>

        <div class="form-group">
            <label for="pais">Pais</label>
            <select name="pais" id="pais" v-model="selected.Pais" class="form-control input-sm" @input="validatePelicula()">

                <option value="" selected disabled hidden>Escoja país</option>
                <option>España</option>
                <option>Estados Unidos</option>
                <option>Francia</option>
                <option>Japón</option>
                <option>China</option>
            </select>
        </div>
        <div class="form-group">
            <label for="duracion">Duración</label>
            <input name="duracion" type="number" step="1" v-model="selected.Duracion" class="form-control input-sm" @input="validatePelicula($event)" @keyup="validatePelicula($event)" placeholder="Introduce duración">
        </div>

        <div class="form-group">
            <h5>
                Género
            </h5>
            <div class="containerCheck" v-for="(item, index) in generoList" :key="index">
                <label for="genero" class="radio-inline"><input type="radio" name="genero" :value="item.value" v-model="selected.Genero" @click="validatePelicula()">{{item.name}}</label>
            </div>
        </div>

        <div class="form-group">
            <label for="titulo">Director</label>
            <input name="titulo" type="text" v-model="selected.Director" class="form-control input-sm" placeholder="Introduce director" @input="validatePelicula()" @keyup="validatePelicula()" title="Escribe un nombre de máximo 40 caracteres">
        </div>

        <div class="form-group">
            <label for="sinopsis">Sinopsis</label>
            <textarea name="sinopsis" v-model="selected.Sinopsis" class="form-control input-sm noresize" placeholder="Introduce sinopsis"></textarea>
        </div>

        <div id="botones">
            <button class="btn btn-success" @click="addPelicula" :disabled="!newMode || !isValid">
                <span class="glyphicon glyphicon-ok"></span>Añadir
            </button>
            <button class="btn btn-warning" @click="updatePelicula" :disabled="newMode || !isValid">
                <span class="glyphicon glyphicon-refresh"></span>Actualizar
            </button>
            <button class="btn btn-danger" @click="deletePelicula" :disabled="newMode || !isValid">
                <span class="glyphicon glyphicon-trash"></span>Borrar
            </button>
        </div>

    </section>
</template>

<script>

export default {
    name: 'detail',
    props: ['selected', 'newMode'],
    data() {
        return {
            isValid: false,
            generoList: [{ name: "Acción", value: "accion" }, { name: "Ciencia ficción", value: "sci-fi" }, { name: "Documental", value: "documental" }, { name: "Comedia", value: "comedia" }]
        }
    },
    methods: {
        addPelicula() {
            this.$emit("callAddPelicula", this.selected);//[args] en array para varios args
            this.resetSelected();
        }, updatePelicula() {
            var actualizacion = window.confirm("¿Está seguro de realizar la actualización?")
            if (actualizacion) {
                this.$emit("callUpdatePelicula", this.selected);
                this.resetSelected();
            }
        }, deletePelicula() {
            var borrado = window.confirm("¿Está seguro de realizar el borrado?")
            if (borrado) {
                this.$emit("callDeletePelicula", this.selected);
                this.resetSelected();
            }
        }, resetSelected: function() {
            this.$emit('nueva');
        }, validatePelicula() {
            var _isValid = true;
            if (this.selected.Titulo.length <= 0 || this.selected.Titulo.length > 40) _isValid = false;
            if (!this.selected.Pais) _isValid = false;
            if (!this.selected.Duracion) _isValid = false;
            if (!this.selected.Genero) _isValid = false;
            if (this.selected.Director.length <= 0 || this.selected.Director.length > 40) _isValid = false;
            if (this.selected.Sinopsis.length <= 0 || this.selected.Titulo.length > 200) _isValid = false;
            this.isValid = _isValid;
        }
    }, updated() {
        this.validatePelicula();
    }
}

</script>

<style scoped>
button:not(:disabled) {
    font-weight: bold;
}

span.glyphicon {
    margin-right: 5px;
}

.containerCheck {
    display: inline-block;
    padding-right: 1em;
}

h5 {
    font-weight: bold;
    margin: 0;
}

.noresize {
    resize: none;
}
</style>
