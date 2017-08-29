<template>
    <nav id="navigator" class="navbar navbar-inverse">
        <div class="container-fluid">
            <div class="navbar-header">
                <button type="button" class="navbar-toggle" data-toggle="collapse" data-target="#myNavbar">
                    <span class="icon-bar"></span>
                    <span class="icon-bar"></span>
                    <span class="icon-bar"></span>
                </button>
                <a class="navbar-brand" href="#">Cines CIC</a>
            </div>
            <div class="collapse navbar-collapse" id="myNavbar">
                <ul class="nav navbar-nav">
                    <li v-for="section in sections" @click="triggerSelection(section,$event)">
                        <a>{{section.titulo}}</a>
                    </li>
                </ul>
            </div>
        </div>
    </nav>
</template>

<script>

export default {
    name: 'navigator',
    data() {
        return {
            sections: [{ name: "entradas", titulo: "Entradas" }, { name: "peliculas", titulo: "Películas" }]
        };
    },
    methods: {
        triggerSelection(clickedSelection, event) {
            let section = clickedSelection;
            $(".active").removeClass("active");
            if (!section) {
                section = {};
                section.name = this.sections[0].name;
                $("nav li").eq(0).addClass("active");
            } else {
                console.log($(event.target));
                $(event.target).parent().addClass("active");
            }
            this.$emit("sectionClicked", section.name);//[args] en array para varios
        }
    },
    mounted() {
        this.triggerSelection();
    }
}

</script>

<style scoped>
* {
    background-color: #337ab7;
    color: #FFF;
}

nav {
    border-radius: 0px;
}

.navbar-inverse .navbar-brand,
.navbar-inverse .navbar-nav>li>a {
    color: #FFF;
}

.navbar-inverse .navbar-nav>.active>a {
    background-color: #286090;
}

.active,.navbar-inverse .navbar-nav>li>a:hover,.navbar-brand:hover {
/*     background-color: #FFF;
    font-weight: bold;
    color:#286090; */
}

.navbar-brand {
    font-family: ubuntu;
    color: #FFF;
}
</style>
