<template>
    <section id="detail" class="col-xs-7 col-sm-7">

        <div class="form-group" :class="[inputs.pelicula?'has-success':'has-error']">
            <label for="pelicula">Película</label>
            <input name="pelicula" type="text" v-model="selected.Pelicula" class="form-control" placeholder="Introduce película" @input="validateEntrada()" @keyup="validateEntrada()" title="Escribe un título de 20 caracteres">
            <div class="message"><span :class="[inputs.pelicula?'hidden':'error']">Escriba como máximo 40 caracteres</span></div>
        </div>
        <div class="form-group" :class="[inputs.fila?'has-success':'has-error']">
            <label for="fila">Fila</label>
            <input name="fila" type="number" v-model="selected.Fila" class="form-control" @input="validateEntrada()" @keyup="validateEntrada()" placeholder="Introduce fila">
            <div class="message"><span :class="[inputs.fila?'hidden':'error']">Número máximo de fila: 15</span></div>
        </div>
        <div class="form-group" :class="[inputs.butaca?'has-success':'has-error']">
            <label for="butaca">Butaca</label>
            <input name="butaca" type="number" v-model="selected.Butaca" class="form-control" @input="validateEntrada()" @keyup="validateEntrada()" placeholder="Introduce butaca">
            <div class="message"><span :class="[inputs.butaca?'hidden':'error']">Número máximo de butaca: 25</span></div>
        </div>
        <div class="form-group" :class="[inputs.precio?'has-success':'has-error']">
            <label for="precio">Precio (€)</label>
            <input name="precio" type="number" step="1" v-model="selected.Precio" class="form-control" @input="validateEntrada()" @keyup="validateEntrada()" placeholder="Introduce precio">
            <div class="message"><span :class="[inputs.precio?'hidden':'error']">Importe máximo 30€</span></div>
        </div>
        <div class="checkbox">
            <label><input name="online" type="checkbox" v-model="selected.Online" @input="validateEntrada()">Online</label>
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
            isValid: false,
            inputs: {
                pelicula: true,
                fila: true,
                butaca: true,
                precio: true,
                online: true
            }
        }
    },
    watch: {
        inputs: function(newQuestion) {
            this.validateEntrada();
        }
    },
    methods: {
        addEntrada() {
            this.$emit("callAddEntrada", this.selected);//[args] en array para varios args
            this.resetSelected();
        }, updateEntrada() {
            var actualizacion = window.confirm("¿Está seguro de realizar la actualización?")
            if (actualizacion) {
                this.$emit("callUpdateEntrada", this.selected);
                this.resetSelected();
            }
        }, deleteEntrada() {
            var borrado = window.confirm("¿Está seguro de realizar la actualización?")
            if (borrado) {
                this.$emit("callDeleteEntrada", this.selected);
                this.resetSelected();
            }
        }, resetSelected: function() {
            this.$emit('nueva');
        }, validateEntrada() {
            var _isValid = true;
            this.inputs.pelicula = this.selected.Pelicula.length > 0 && this.selected.Pelicula.length <= 40;
            this.inputs.fila = this.selected.Fila > 0 && this.selected.Fila <= 15;
            this.inputs.butaca = this.selected.Butaca > 0 && this.selected.Butaca <= 25;
            this.inputs.precio = this.selected.Precio > 0 && this.selected.Precio <= 30

            for (var key in this.inputs) {
                if (!this.inputs[key]) _isValid = false;
            }
            this.isValid = _isValid;
        }
    },
    updated() {
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

.error{
    color:red;
}

.message{
    height: 14px;
}
</style>
