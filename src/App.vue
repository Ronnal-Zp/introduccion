
<script setup>
  import { ref, onMounted } from "vue";
  import { db } from "./data/guitarras";
  import Guitarra from "./components/Guitarra.vue"
  import Header from "./components/Header.vue"
  import Footer from "./components/Footer.vue"

  const guitarras = ref([]);
  const carrito = ref([]);

  onMounted(() => {
    guitarras.value = db;
  });

  const onAgregarCarrito = (guitarra) => {
    const existsGuitarra = carrito.value.findIndex(g => g.id == guitarra.id);
    
    if(existsGuitarra >= 0 ) {
        carrito.value[existsGuitarra].cantidad += 1;
    } else {
        guitarra.cantidad = 1;
        carrito.value.push(guitarra);
    }
  }

  const aumentarCantidad = (id) => {
    const index = carrito.value.findIndex(g => g.id == id);

    if(index >= 0 ) {
        carrito.value[index].cantidad += 1;
    }
  }

  const disminuirCantidad = (id) => {
    const index = carrito.value.findIndex(g => g.id == id);

    if(index >= 0 && carrito.value[index].cantidad > 1) {
        carrito.value[index].cantidad -= 1;
    }
  }
  

</script>

<template>
    <Header
        :carrito="carrito"
        @aumentar-cantidad="aumentarCantidad"
        @disminuir-cantidad="disminuirCantidad"
    />

    <main class="container-xl mt-5">
        <h2 class="text-center">Nuestra Colección</h2>

        <div  class="row mt-5">
            <Guitarra 
                v-for="guitarra in guitarras"
                :guitarra="guitarra"
                @agregar-carrito="onAgregarCarrito"
            />
        </div>
    </main>

    <Footer/>
</template>