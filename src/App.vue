<script setup>
 import { ref, reactive,onMounted } from "vue";
 import { db } from './data/guitarras';
 import Guitarra from "./components/guitarra.vue";
 import Header from "./components/Header.vue";
 import Footer from "./components/Footer.vue";
 /* const state = reactive({
     guitarras: db
 }) */

 const guitarras = ref([]);
 const carrito = ref([]);
 onMounted(()=>{
    guitarras.value = db;
 });
 const agregarCarreti = (guitarra) => {
    carrito.value.push(guitarra);
 }
</script>

<template>
    <Header
    :carrito="carrito"
    />
    <main class="container-xl mt-5">
        <h2 class="text-center">Nuestra Colección</h2>

        <div class="row mt-5">
                  <Guitarra 
                    v-for="guitarra in guitarras" :key="guitarra.id"
                    :guitarra="guitarra"
                    @incrementar="agregarCarreti"
                  />
        </div>
    </main>
    <Footer/>
</template>
