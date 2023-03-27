<script setup>
    import {ref,computed, watch} from 'vue'
    import Header from './components/Header.vue'
    import Button from './components/Button.vue'
    import { totalDineroPagar } from  './helpers/index.js';


    const cantidad = ref(10000)
    const meses = ref(6)
    const total = ref(0)
    const MIN = 0
    const MAX = 20000
    const STEP = 100

    const formatearDInero = (valor) => {
        const formatter = new Intl.NumberFormat('en-US', {
            style:'currency',
            currency:'USD'
        })
        return formatter.format(valor)
    }

    watch([cantidad, meses] , () => {
        total.value = totalDineroPagar(cantidad.value, meses.value)
    })

    const pagoMensual = computed(() =>{
        return total.value / meses.value
    })

    const handleChangeDecremento = () => {
        const valor = cantidad.value - STEP
        if(valor < MIN){
            alert('Cantidad no valida')
            return
        }
        cantidad.value = valor
    }

    const handleChangeIncremento = () => {
        const valor = cantidad.value + STEP
        if(valor > MAX){
            alert('Cantidad no valida')
            return
        }
        cantidad.value = valor
    }

    
</script>

<template>
 <div class="bg-white my-20 max-w-lg mx-auto shadow p-10">
    <Header/>

    <div class="flex justify-between my-6"> 
       <Button
       :operador="'-'"
       @fn="handleChangeDecremento"
       />

       <Button
       :operador="'+'"
       @fn="handleChangeIncremento"
       />
    </div>

    <div class="my-5">
        <input 
        type="range" 
        class="w-full bg-gray-200 accent-lime-500 hover:accent-lime-600"
        @input="handleChange"
        :min="MIN"
        :max="MAX"
        :step="STEP"
        v-model.number="cantidad"
        />
        <p class="text-indigo-600 text-center my-10 text-5xl font-extrabold">
          {{formatearDInero(cantidad)}}</p>


        <h2 class="text-2xl font-extrabold text-gray-500 text-center">
            Elige un <span className="text-indigo-600">Plazo</span> a pagar
        </h2>

        <select name="" id=""
        class="mt-5 w-full p-2 bg-white border border-gray-300 rounded-lg text-center
        text-xl font-bold text-gray-500"
        :value="meses"
        v-model.number="meses"
        >
          <option value="6">6 Meses</option>
          <option value="12">12 Meses</option>
          <option value="24">24 Meses</option>

        </select>

        <div v-if="total > 0" class="my-5 space-y-3 bg-gray-50 p-5">
            <h2 class="text-2xl font-extrabold text-gray-500 text-center">
              Resumen <span class="text-indigo-600">de pagos</span>
            </h2>
            <p class="text-xl text-gray-500 text-center font-bold">{{meses}}Meses</p>
            <p class="text-xl text-gray-500 text-center font-bold">Total a pagar: {{formatearDInero(total) }}</p>
            <p class="text-xl text-gray-500 text-center font-bold">Mensuales: {{formatearDInero(pagoMensual)}}</p>

        </div>
        <p v-else class=" my-5 text-xl text-center text-gray-500  font-bold">Añade una cantidad y un plazo a pagar</p>
    </div>
 </div>
</template>

