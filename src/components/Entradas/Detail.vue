<template>
    <section id="detail" class="col-xs-7 col-sm-7">

        <div class="form-group">
            <label for="pelicula">Película</label>
            <input name="pelicula" type="text" v-model="selected.Pelicula" class="form-control" placeholder="Introduce película" @change="validateEntrada" title="Escribe un título de 20 caracteres">
        </div>
        <div class="form-group">
            <label for="fila">Fila</label>
            <input name="fila" type="number" v-model="selected.Fila" class="form-control" @change="validateEntrada" placeholder="Introduce fila">
        </div>
        <div class="form-group">
            <label for="butaca">Butaca</label>
            <input name="butaca" type="number" v-model="selected.Butaca" class="form-control" @change="validateEntrada" placeholder="Introduce butaca">
        </div>
        <div class="form-group">
            <label for="precio">Precio</label>
            <input name="precio" type="number" step="0.01" v-model="selected.Precio" class="form-control" @change="validateEntrada" placeholder="Introduce precio">
        </div>
        <div class="checkbox">
            <label><input name="online" type="checkbox" v-model="selected.Online" @change="validateEntrada">Online</label>
        </div>

        <div id="botones">
            <button class="btn btn-success" @click="addEntrada" :disabled="!newMode || !isValid">
                <span class="glyphicon glyphicon-ok"></span>Añadir
            </button>
            <button class="btn btn-warning" @click="updateEntrada" :disabled="newMode || !isValid">
                <span class="glyphicon glyphicon-refresh"></span>Actualizar
            </button>
            <button class="btn btn-danger" @click="deleteEntrada" :disabled="newMode || !isValid">
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
            isValid: false
        }
    },
    methods: {
        addEntrada() {
            this.selected.Fila = parseInt(this.selected.Fila);
            this.selected.Butaca = parseInt(this.selected.Butaca);
            this.selected.Precio = parseInt(this.selected.Precio);
            this.$emit("callAddEntrada", this.selected);//[args] en array para varios args
            this.resetSelected();
        }, updateEntrada() {
            this.selected.Id = parseInt(this.selected.Id);
            this.selected.Fila = parseInt(this.selected.Fila);
            this.selected.Butaca = parseInt(this.selected.Butaca);
            this.selected.Precio = parseInt(this.selected.Precio);
            this.$emit("callUpdateEntrada", this.selected);
            this.resetSelected();
        }, deleteEntrada() {
            this.$emit("callDeleteEntrada", this.selected);
            this.resetSelected();
        }, resetSelected: function() {
            this.$emit('nueva');
        }, validateEntrada() {
            var _isValid = true;
            if (!(this.selected.Pelicula.length > 0 && this.selected.Pelicula.length <= 40)) _isValid = false;
            if (!(this.selected.Fila > 0 && this.selected.Fila <= 15)) _isValid = false;
            if (!(this.selected.Butaca > 0 && this.selected.Butaca <= 25)) _isValid = false;
            if (!(this.selected.Precio > 0 && this.selected.Precio <= 30)) _isValid = false;
            if (this.selected.Online == undefined && this.selected.Online == null) _isValid = false;
            this.isValid = _isValid;
            console.log(_isValid);
        }
    }, updated() {
        this.validateEntrada();
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
</style>
