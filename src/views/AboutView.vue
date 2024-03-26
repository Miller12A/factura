<template>
  <div class="about">
    <h1 class="text-4xl font-extrabold text-black">Facturas guardadas</h1>
  </div>
  <section>
    <div>
      <section>
        <div class="relative overflow-x-auto px-40 mt-7">
          <table class="w-full text-sm text-left rtl:text-right text-gray-500 dark:text-gray-400">
            <thead class="text-xs text-gray-700 uppercase bg-gray-100 dark:bg-gray-700 dark:text-gray-400">
              <tr>
                <th scope="col" class="px-6 py-3">
                  Nombre de factura
                </th>
                <th scope="col" class="px-6 py-3">
                  
                </th>
                <th scope="col" class="px-6 py-3">
                  fecha
                </th>
                <th scope="col" class="px-6 py-3">
                  ACCION
                </th>
              </tr>
            </thead>
            <tbody>
              <tr class="bg-white dark:bg-gray-800" >
                <th scope="row" class="px-6 py-4 font-medium text-gray-900 whitespace-nowrap dark:text-white">
                  <input class="bg-white dark:bg-gray-800" type="text"  placeholder="ejemplo.pdf">
                </th>
                <td class="px-6 py-4">
                  
                </td>
                <td class="px-6 py-4">
                  <input class="bg-white dark:bg-gray-800" type="number"  placeholder="10/05/2004">
                </td>
                <td class="px-6 py-4">
                  <button type="button" class="focus:outline-none text-white bg-red-700 hover:bg-red-800 focus:ring-4 focus:ring-red-300 font-medium rounded-lg text-sm px-5 py-2.5 me-2 mb-2 dark:bg-red-600 dark:hover:bg-red-700 dark:focus:ring-red-900">Eliminar</button>
                  <button type="button" class="focus:outline-none text-white bg-yellow-400 hover:bg-yellow-500 focus:ring-4 focus:ring-yellow-300 font-medium rounded-lg text-sm px-5 py-2.5 me-2 mb-2 dark:focus:ring-yellow-900">Descargar</button>
                </td>
              </tr>
              <tr class="bg-white dark:bg-gray-800" >
                <th scope="row" class="px-6 py-4 font-medium text-gray-900 whitespace-nowrap dark:text-white">
                  <input class="bg-white dark:bg-gray-800" type="text"  placeholder="ejemplo.pdf">
                </th>
                <td class="px-6 py-4">
                  
                </td>
                <td class="px-6 py-4">
                  <input class="bg-white dark:bg-gray-800" type="number"  placeholder="10/05/2004">
                </td>
                <td class="px-6 py-4">
                  <button type="button" class="focus:outline-none text-white bg-red-700 hover:bg-red-800 focus:ring-4 focus:ring-red-300 font-medium rounded-lg text-sm px-5 py-2.5 me-2 mb-2 dark:bg-red-600 dark:hover:bg-red-700 dark:focus:ring-red-900">Eliminar</button>
                  <button type="button" class="focus:outline-none text-white bg-yellow-400 hover:bg-yellow-500 focus:ring-4 focus:ring-yellow-300 font-medium rounded-lg text-sm px-5 py-2.5 me-2 mb-2 dark:focus:ring-yellow-900">Descargar</button>
                </td>
              </tr>
            </tbody>
          </table>
        </div>
          </section>
    </div>
  </section>
</template>


<script setup lang="ts">

</script>