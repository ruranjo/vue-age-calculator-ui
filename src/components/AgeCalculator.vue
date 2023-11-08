<script lang="ts" setup>
    import { ref, watch } from 'vue';

    const day = ref(1);
    const month = ref(1);
    const year = ref(2000);
    const acountMonthDays = [31,28,31,30,31,30,31,31,30,31,30,31];
    const daylimit = ref(31);
    const dayout = ref(0);
    const monthout = ref(0);
    const yearout = ref(0);

    const calculateAgeFromDateOfBirth = () => {
        
        // Dos cadenas de fecha
        const first = month.value >= 10  ? year.value+"-"+month.value+"-"+day.value : year.value+"-0"+month.value+"-"+day.value;
        
        // Crea dos objetos Date con las fechas para comparar
        const y = new Date(first);
        const x = new Date();

        console.log(y);
        console.log(x);
        
        //Obtiene la diferencia en milisegundos
        const diff = x.getTime() - y.getTime();
        
        // segundos = milisegundos / 1000
        // minutos = segundos / 60
        // horas = minutos / 60
        // Días = horas / 24
        
        //Convierte la diferencia a días
        
        let days = Math.floor(diff / (1000 * 60 * 60 * 24));
        yearout.value = Math.floor(days/365);
        days = days%365;
        monthout.value = Math.floor(days/30);
        dayout.value = days%12; 
        
    }


    watch([month], ()=>{
        if(day.value > acountMonthDays[month.value - 1] ){
            day.value = 1;    
        }
        daylimit.value = acountMonthDays[month.value - 1];
    })
</script>

<template>
    <div class="card">
        <div class="container">
            <div >
                
                <div class="form_container">
                    <div class="block">
                    <label for="day">
                    Dia
                    </label>
                    <input type="number" v-model="day" placeholder="DD" id="day" min="1" v-bind:max="daylimit">
                    <small></small>
                </div>

                <div class="block">
                    <label for="month">
                    Mes
                    </label>
                    <input type="number" v-model="month" placeholder="MM" id="month" min="1" max="12">
                    <small></small>
                </div>

                <div class="block">
                    <label for="year">
                    Año
                    </label>
                    <input type="number" v-model="year" placeholder="YYYY" id="year"  min="1" >
                    <small></small>
                </div>

                </div>
                <div class="submit_block">
                    <hr>
                    <button v-on:click="calculateAgeFromDateOfBirth" class="sumbit_btn"><img src="../assets/icon-arrow.svg" alt="icon"></button>
                </div>
            </div>

            <div class="output">
                <h1><span id="YY">{{ yearout + " "}} </span>años</h1>
                <h1><span id="MM">{{ monthout + " "}} </span>meses</h1>
                <h1><span id="DD">{{ dayout + " "}} </span>dias</h1>
            </div>
        </div>
    </div>
</template>

<style scoped>
    .card{
        background-color: var(--White);
        width: 600px;
        border-radius: 1rem 1rem 10rem .1rem;
    }
    .container{
        padding: 2rem;
        display: flex;
        flex-direction: column;
    }
    form{
        display: flex;
        flex-direction: column;
    }
    .form_container{
        display: flex;
        
        gap: 2rem;
    }
    .block{
        display: flex;
        flex-direction: column;
    }
    .block label{
        text-transform: uppercase;
        font-weight: 600;
    }
    input{
        width: 100px;
        padding: .5rem;
        border-radius: .5rem;
        border: 1px solid var(--Light-grey);
        display: flex;
        align-items: center;
        font-size: 1.5rem;
    }
    input::placeholder{
        font-size: 1.5rem;
        font-weight: 800;
    }
    .submit_block{
        display: flex;
        align-items: center;
    }
    .submit_block hr{
        width: 100%;
    }
    .sumbit_btn{
        border-radius: 50%;
        padding: 1rem;
        background-color: var(--Purple);
        border: none;
        cursor: pointer;
        display: flex;
        align-items: center;
        justify-content: center;
    }
    .output h1{
        font-size: 5rem;
        font-weight: 800;
        font-style: italic;
        height: fit-content;
    }
    .output span{
        color: var(--Purple);
    }
    small{
        color: red;
    }

    @media screen and (max-width:600px) {
        .card{
            width: 360px;
        }
        .container{
            padding: 1rem;
        }
        .form_container{
            margin-bottom: 5rem;
            gap: 0;
            justify-content: space-between;
        }
        .submit_block{
            position: relative;
        }
        .sumbit_btn{
            position: absolute;
            top: 50%;
            left: 50%;
            transform: translate(-50%,-50%);
        }
        .output{
            margin-top: 5rem;
        }
        .output h1{
            font-size: 3.5rem;
        }
    }
</style>