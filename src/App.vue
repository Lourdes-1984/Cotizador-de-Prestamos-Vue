<script setup>
    import {ref,computed} from 'vue'
    import Header from './components/Header.vue'
    import Button from './components/Button.vue'


    const cantidad = ref(10000)
    const MIN = 0
    const MAX = 20000
    const STEP = 100

    const formatearDInero = computed(() => {
        const formatter = new Intl.NumberFormat('en-US', {
            style:'currency',
            currency:'USD'
        })
        return formatter.format(cantidad.value)
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
       :fn="handleChangeDecremento"
       />

       <Button
       :operador="'+'"
       :fn="handleChangeIncremento"
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
          {{formatearDInero}}</p>
    </div>
 </div>
</template>

