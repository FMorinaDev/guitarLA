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
 const guitarra = ref({});
 onMounted(()=>{
    guitarras.value = db;
    guitarra.value = db[3];
 });
 const agregarCarreto = (guitarra) => {
    const existeCarrito = carrito.value.findIndex(producto => producto.id === guitarra.id);
    if (existeCarrito >= 0) {
        carrito.value[existeCarrito].cantidad++;
    }else{
        guitarra.cantidad = 1;
        carrito.value.push(guitarra);
    }
 }
 const vaciarCarrito = () => {
    carrito.value = [];
 }
 const incrementarCantidad = (id) => {
     const index = carrito.value.findIndex(producto => producto.id === id);
     carrito.value[index].cantidad++;
 }
 const decrementarCantidad = (id) => {
     const index = carrito.value.findIndex(producto => producto.id === id);
     carrito.value[index].cantidad--;
     if (carrito.value[index].cantidad == 0) {
        quitarGuitarra(carrito.value[index].id);
     }
     
 }
 const quitarGuitarra = (id) => {
    carrito.value = carrito.value.filter( producto => producto.id !== id);

 }
</script>

<template>
    <Header
        :carrito="carrito" 
        :guitarra="guitarra"   
        @vaciar="vaciarCarrito"
        @menos="decrementarCantidad"
        @mas="incrementarCantidad"
        @quitar="quitarGuitarra"
        @incrementar="agregarCarreto"
    />
    <main class="container-xl mt-5">
        <h2 class="text-center">Nuestra Colección</h2>

        <div class="row mt-5">
                  <Guitarra 
                    v-for="guitarra in guitarras" :key="guitarra.id"
                    :guitarra="guitarra"
                    @incrementar="agregarCarreto"
                  />
        </div>
    </main>
    <Footer/>
</template>
