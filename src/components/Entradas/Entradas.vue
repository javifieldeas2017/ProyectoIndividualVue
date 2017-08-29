<template>
    <div id="entradas">
        <master :entradas="entradas" @nueva="unsetSelected" @selected="setSelected"></master>
        <detail :newMode="newMode" :selected="selected" @callAddEntrada="addEntrada" @callUpdateEntrada="updateEntrada" @callDeleteEntrada="deleteEntrada"></detail>
    </div>
</template>

<script>
import master from './Master.vue'
import detail from './Detail.vue'
export default {
    name: 'entradas',
    data() {
        return {
            entradas: [],
            newMode: true,
            selected: null
        };
    },
    components: {
        master, detail
    },
    methods: {
        getEntradas: function() {
            let _this = this;
            $.ajax({
                type: 'GET',
                url: 'http://localhost:51398/api/Entradas/',
                success: function(response) {
                    _this.entradas = response;
                }
            });
        },
        addEntrada: function(entrada) {
            let _this = this;
            $.ajax({
                type: 'POST',
                url: 'http://localhost:51398/api/Entradas/',
                data: entrada,
                success: function(response) {
                    _this.entradas.push(response);
                    _this.unsetSelected();
                },
                error: this.error
            });
        },
        updateEntrada: function(entrada) {
            let _this = this;
            $.ajax({
                type: 'PUT',
                url: 'http://localhost:51398/api/Entradas/' + entrada.Id,
                data: entrada,
                success: function(response) {
                    var index = _this.entradas.findIndex((el) => el.Id == entrada.Id);
                    _this.entradas[index] = entrada;
                    _this.entradas = JSON.parse(JSON.stringify(_this.entradas))
                    _this.unsetSelected();
                },
                error: this.error
            });
        },
        deleteEntrada: function(entrada) {
            let _this = this;
            $.ajax({
                type: 'DELETE',
                url: 'http://localhost:51398/api/Entradas/' + entrada.Id,
                success: function(response) {
                    var index = _this.entradas.findIndex((el) => el.Id == entrada.Id);
                    _this.entradas.splice(index, 1);
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
            this.selected = { Pelicula: null, Fila: null, Butaca: null, Precio: null, Online: false }
        },
        setSelected: function(entrada) {
            this.newMode = false;
            this.selected = JSON.parse(JSON.stringify(entrada));
        }

    },
    created() {
        this.getEntradas();
        this.unsetSelected();
    }
}

</script>

<style scoped>

</style>
