<script setup>
import { RouterLink, RouterView } from "vue-router";
import { ref } from 'vue'

let showWeekOptions = ref(false);
let showDayOptions = ref(false);
let selectedWeek = ref('')
let selectedDay = ref('o')
// let selectedDay = useTemplateRef('selectedDay')

</script>

<template>
  <div class="w-full h-full flex flex-row relative">
    <!-- sidebar -->
    <div class="h-full fixed left-0 top-0 w-[50px] bg-zinc-900"></div> 
    <!-- inputs -->
    <div
      class="min-h-[100vh] w-full absolute top-0 left-[50px] py-[100px] px-[50px] flex flex-col justify-center relative"
    >
      <!-- inputs container -->
      <div class="grid grid-rows-3 gap-[20px] relative">
        <div class="flex flex-col relative">
          <label>Неделя</label>
          <!-- <select class="select" @input="profile">
            <option disabled></option>
            <option>ИСиП</option>
            <option>ПИвЭиУ</option>
            <option>ИБ</option>
            <option>ИСиТ</option>
            <option>РПИС</option>
            <option>РПОиПИИ</option>
            <option>ПОВТиАС</option>
          </select> -->
          <div @click="showWeekOptions = !showWeekOptions" class="bg-white rounded w-[500px] p-[8px] h-[50px] cursor-pointer"></div>
          <!-- <div v-if="showProfileOptions" class="bg-white w-[500px] p-[8px] rounded absolute bottom-[-230px] border-[1px] border-black" @input="profile">
            <div class="option">ИСиП</div>
            <div class="option">ПИвЭиУ</div>
            <div class="option">ИБ</div>
            <div class="option">ИСиТ</div>
            <div class="option">РПИС</div>
            <div class="option">РПОиПИИ</div>
            <div class="option">ПОВТиАС</div>
          </div> -->
          <div v-if="showWeekOptions" class="options" @input="profile">
            <div @click="selectedWeek = '1'" class="option">1</div>
            <div @click="selectedWeek = '2'" class="option">2</div>
          </div>
        </div>

        <div class="flex flex-col relative">
          <label>День недели</label>
          <div @click="showDayOptions = !showDayOptions" class="bg-white rounded w-[500px] p-[8px] h-[50px] cursor-pointer" value="selectedDay"></div>
          <div v-if="showDayOptions" class="options" @click="showDayOptions = !showDayOptions" @input="profile">
            <div class="option">Вся неделя</div>
            <div class="option">Понедельник</div>
            <div class="option">Вторник</div>
            <div class="option">Среда</div>
            <div class="option">Четверг</div>
            <div class="option">Пятница</div>
            <div class="option">Суббота</div>
          </div>
        </div>

        <!-- <div class="flex flex-col">
          <label>Курс</label>
          <select class="select" @input="course">
            <option disabled></option>
            <option>1</option>
            <option>2</option>
            <option>3</option>
            <option>4</option>
          </select>
        </div>

        <div class="flex flex-col">
          <label>Неделя</label>
          <select class="select" @input="week">
            <option disabled></option>
            <option>Первая</option>
            <option>Вторая</option>
          </select>
        </div> -->
        <button class="uppercase rounded-full bg-blue-800 w-fit px-[50px] py-[10px] text-zinc-300 shadow-md hover:opacity-[.8] absolute bottom-0 transition-[.3s] outline-none">get</button>
      </div>
    </div>
    <div class="">{{ selectedDay }}</div>
  </div>
</template>