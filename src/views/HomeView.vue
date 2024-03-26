<template>
  <div>
    <div class="w-full h-full flex justify-center items-center">
      <h1 id="typewriter" class="text-4xl font-bold"></h1>
    </div>
    <h1 class="text-4xl font-extrabold text-black">Facturacion</h1>
    <div class="mt-12 mb-2">
      <button @click="nuevaFactura" type="button" class="focus:outline-none text-white bg-green-700 hover:bg-green-800 focus:ring-4 focus:ring-green-300 font-medium rounded-lg text-sm px-5 py-2.5 me-2 mb-2 dark:bg-green-600 dark:hover:bg-green-700 dark:focus:ring-green-800">Nueva Factura</button>
      <button @click="guardarFactura" type="button" class="focus:outline-none text-white bg-yellow-400 hover:bg-yellow-500 focus:ring-4 focus:ring-yellow-300 font-medium rounded-lg text-sm px-5 py-2.5 me-2 mb-2 dark:focus:ring-yellow-900">Guardar Factura</button>
      <button @click="addProduct" class="mt-4 mb-4 px-4 py-2 mr-1 bg-blue-500 text-white rounded-md shadow hover:bg-blue-600 focus:outline-none focus:ring-2 focus:ring-blue-500 focus:ring-offset-2">Agregar Producto</button>
          <button @click="generatePDF" class="mt-4 mb-4 px-4 py-2 bg-orange-500 text-white rounded-md shadow hover:bg-orange-600 focus:outline-none focus:ring-2 focus:ring-orange-500 focus:ring-offset-2">Imprimir Factura</button>
    </div>
  </div>

  <div id="invoice" class="bg-white rounded-lg shadow-lg px-8 py-10 max-w-xl mx-auto">
    <div class="flex items-center justify-between mb-8">
        <div class="flex items-center">
            <img class="h-8 w-8 mr-2" src="https://tailwindflex.com/public/images/logos/favicon-32x32.png"
                alt="Logo" />
            <div class="text-gray-700 font-semibold text-lg">PC GERENTE</div>
        </div>
        <div class="text-gray-700">
            <div class="font-bold text-xl mb-2">INVOICE</div>
            <div class="text-sm"><input class="text-center" type="date" placeholder="Fecha"></div>
            <div class="text-sm">Invoice #: <input class="" type="text"></div>
        </div>
    </div>
    <div class="border-b-2 border-gray-300 pb-8 mb-8">
        <h2 class="text-2xl font-bold mb-4">Cliente:</h2>
        <div class="text-gray-700 mb-2"><input class="text-center" type="text" placeholder="Ingresar Cliente"></div>
        <div class="text-gray-700 mb-2"><input class="text-center" type="text" placeholder="Ingresar Direccion"></div>
        <div class="text-gray-700"><input class="text-center" type="text" placeholder="Ingresar Correo"></div>
    </div>
    <table class="w-full text-left mb-8">
        <thead>
            <tr>
                <th class="text-gray-700 font-bold uppercase py-2">Producto</th>
                <th class="text-gray-700 font-bold uppercase py-2">Cantidad</th>
                <th class="text-gray-700 font-bold uppercase py-2">Precio Unitario</th>
                <th class="text-gray-700 font-bold uppercase py-2">Precio Total</th>
                <th class="text-gray-700 font-bold uppercase py-2">+ IVA</th>
            </tr>
        </thead>
        <tbody>
            <tr  v-for="(product, index) in products" :key="index">
                <td class="py-4 text-gray-700">
                  <input class="w-[7rem]" v-model="product.name" type="text" placeholder="Producto">
                </td>
                <td class="py-4 text-gray-700">
                  <input class="w-[7rem]" type="text" v-model="product.quantity" @input="calculateTotal(product)">
                </td>
                <td class="py-4 text-gray-700">
                  <input class="w-[7rem]" type="text" v-model="product.unitPrice" @input="calculateTotal(product)">
                </td>
                <td class="py-4 text-gray-700">
                  <input class="w-[7rem]" type="text" :value="formatCurrency(product.totalPrice)" disabled>
                </td>
                <td class="py-4 text-gray-700">
                  <input class="w-[7rem]" type="checkbox" v-model="product.applyIVA" @change="updateTotalPrice(product)">
                </td>
            </tr>
        </tbody>
    </table>
    <div class="flex justify-end mb-8">
        <div class="text-gray-700 mr-2">Subtotal:</div>
        <div class="text-gray-700">${{ formatCurrency(totalPriceSum) }}</div>
    </div>
    <div class="text-right mb-8">
        <div class="text-gray-700 mr-2">Total + IVA:</div>
        <div class="text-gray-700">${{ formatCurrencyWithIVA(totalPriceSum) }}</div>

    </div>
    <div class="flex justify-end mb-8">
        <div class="text-gray-700 mr-2">Total:</div>
        <div class="text-gray-700 font-bold text-xl">${{ formatCurrencyWithIVA(totalPriceSum) }}</div>
    </div>
    <div class="border-t-2 border-gray-300 pt-8 mb-8">
        <div class="text-gray-700 mb-2">Con esta factura tiene opcion a reclamo dentro de los primeros 30 dias.</div>
        <div class="text-gray-700 mb-2">Estamos ubicados en:</div>
        <div class="text-gray-700">García Avilés 408 y Luque, Edificio FINEC, piso 7, oficina 723</div>
    </div>
</div>
</template>


<script setup lang="ts">
import { ref } from 'vue';
import html2pdf from 'html2pdf.js';


interface Product {
  name: string;
  quantity: number;
  unitPrice: number;
  totalPrice: number;
  applyIVA: boolean;
}

const products = ref<Product[]>([
  { name: '', quantity: 0, unitPrice: 0, totalPrice: 0, applyIVA: false}
]);

let totalPriceSum = ref<number>(0);

const addProduct = () => {
  products.value.push({ name: '', quantity: 0, unitPrice: 0, totalPrice: 0, applyIVA: false });
};

const calculateTotal = (product: Product) => {
  product.totalPrice = product.quantity * product.unitPrice;
  updateTotalPriceSum();
};

const updateTotalPrice = (product: Product) => {
  // Si el checkbox está marcado, se suma el 12% de IVA al total
  if (product.applyIVA) {
    product.totalPrice *= 1.12;
  } else {
    product.totalPrice /= 1.12; // Si se desmarca, eliminamos el 12% de IVA
  }
  updateTotalPriceSum();
};

const updateTotalPriceSum = () => {
  totalPriceSum.value = products.value.reduce((acc, product) => acc + product.totalPrice, 0);
};


const formatCurrency = (value: number) => {
  return value.toFixed(2);
};

const formatCurrencyWithIVA = (value: number) => {
  return (value * 1.12).toFixed(2); // Multiplicamos el valor por 1.12 antes de redondearlo
};

const generatePDF = () => {
  const element = document.getElementById('invoice'); // El ID de tu contenedor principal en el HTML
  const options = {
    filename: 'factura.pdf', // Nombre del archivo PDF
    image: { type: 'jpeg', quality: 0.90 }, // Calidad de la imagen (0 a 1)
    html2canvas: { scale: 5 }, // Escala del lienzo HTML
    jsPDF: { 
      unit: 'pt', 
      format: 'a4', 
      orientation: 'landscape'
    }
  };

  html2pdf()
    .from(element)
    .set(options)
    .save();
};

const nuevaFactura = () => {
  products.value = [{ name: '', quantity: 0, unitPrice: 0, totalPrice: 0, applyIVA: false }];
};

const guardarFactura = () => {
  //const response = await axios.post('/crear-factura', products.value);
    //if (response.status === 200) {
  // Limpiar los campos de la factura después de guardar
  nuevaFactura();
//}
};

const words = ["Hola, mi nombre es Miller!", "Este es un programa de Factura!", "Espero te sirva!."];
let i = 0;
let j = 0;
let currentWord = "";
let isDeleting = false;

function type() {
  currentWord = words[i];
  if (isDeleting) {
    const typewriterElement = document.getElementById("typewriter");
    if (typewriterElement !== null) {
      typewriterElement.textContent = currentWord.substring(0, j - 1);
    }
    j--;
    if (j === 0) {
      isDeleting = false;
      i++;
      if (i === words.length) {
        i = 0;
      }
    }
  } else {
    const typewriterElement = document.getElementById("typewriter");
    if (typewriterElement !== null) {
      typewriterElement.textContent = currentWord.substring(0, j + 1);
    }
    j++;
    if (j === currentWord.length) {
      isDeleting = true;
    }
  }
  setTimeout(type, 100);
}

type();

</script>



