<template>
    <section id="detail" class="col-xs-7 col-sm-7">

        <div class="form-group">
            <label for="titulo">Título</label>
            <input name="titulo" type="text" v-model="selected.Titulo" class="form-control" placeholder="Introduce película" @input="validatePelicula()" @keyup="validatePelicula()" title="Escribe un título de 20 caracteres">
        </div>

        <div class="form-group">
            <label for="pais">Pais</label>
            <select name="pais" id="pais" v-model="selected.Pais" class="form-control" @input="validatePelicula()">

                <option value="" selected disabled hidden>Escoja país</option>
                <option>España</option>
                <option>Estados Unidos</option>
                <option>Francia</option>
                <option>Japón</option>
                <option>China</option>
            </select>
        </div>
        <div class="form-group">
            <div>
                <b>Duración (minutos)</b>
            </div>
            <label for="one" class="radio-inline"><input type="radio" name="duracion" value="30" v-model="selected.Duracion" @click="validatePelicula()">15</label>
            <label for="one" class="radio-inline"><input type="radio" name="duracion" value="60" v-model="selected.Duracion" @click="validatePelicula()">30</label>
            <label for="one" class="radio-inline"><input type="radio" name="duracion" value="60" v-model="selected.Duracion" @click="validatePelicula()">60</label>
            <label for="one" class="radio-inline"><input type="radio" name="duracion" value="90" v-model="selected.Duracion" @click="validatePelicula()">90</label>
            <label for="one" class="radio-inline"><input type="radio" name="duracion" value="120" v-model="selected.Duracion" @click="validatePelicula()">120</label>

        </div>

        <div class="form-group">
            <div>
                <b>Género</b>
            </div>
            <div class="containerCheck" v-for="(item, index) in itemList" :key="index" >
                <label class="checkbox-inline"><input type="checkbox" value="item.value" v-model="selected.Genero" @click="validatePelicula()" :disabled="selected.Genero.length ==1 && selected.Genero.indexOf(index) == -1">{{item.name}}</label>
            </div>
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
            itemList: [{name:"Acción", value: "accion"},{name:"Ciencia ficción", value: "sci-fi"},{name:"Documental", value: "documental"},{name:"Comedia", value: "comedia"}]
        }
    },
    methods: {
        addPelicula() {
            this.$emit("callAddPelicula", this.selected);//[args] en array para varios args
            this.resetSelected();
        }, updatePelicula() {
            this.$emit("callUpdatePelicula", this.selected);
            this.resetSelected();
        }, deletePelicula() {
            this.$emit("callDeletePelicula", this.selected);
            this.resetSelected();
        }, resetSelected: function() {
            this.$emit('nueva');
        }, validatePelicula() {
            var _isValid = true;
            if (this.selected.Titulo.length <= 0 || this.selected.Titulo.length > 40) _isValid = false;
            if (!this.selected.Pais) _isValid = false;
            if (!this.selected.Duracion) _isValid = false;
            if (this.selected.Genero.length > 1) _isValid = false;
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
    display:inline-block;
    padding-right:1em;
}
</style>
