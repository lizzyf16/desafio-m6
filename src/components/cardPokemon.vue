<template>
    <div class="cardPokemon ">
        <div class="cardPoke">
            <img :class="{filtro: !mostrar}" :src="imgPoke" :alt="nombrePoke">
            <div v-if="mostrar">
                <p>{{ nombrePoke }}</p>
            </div>
            <div v-else>
                <form class="d-flex flex-column gap-1">
                <input type="text" v-model="respuestaPoke">
                <button class="btn btn-success" @click="enviarPoke">Descubrir</button>
            </form>
            </div>

        </div>
    </div>
</template>

<script>
export default {
    name: 'cardPokemon',
    components: {
    },
    props: {
        imgPoke: String,
        nombrePoke: String,
    },
    data() {
        return {
            respuestaPoke: "",
            mostrar: false
        }
    },
    methods:{
        enviarPoke: function(){
            if(this.nombrePoke == this.respuestaPoke){
                this.$emit("descubrir"),
                this.mostrar = !this.mostrar
            } else{
                alert("Ups, te haz equivocado, vuelve a intentarlo")
                
            }
        },
    }
}
</script>

<style scoped>
.cardPoke {

    display: block;
    align-items: center;
    gap: 10px;
}
.filtro{
    filter: blur(5px) grayscale(100%);
}
</style>