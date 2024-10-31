<script setup>
import { RouterLink, RouterView } from "vue-router";
import { ref, computed } from "vue";
import { data } from "./data.js";

let showWeekOptions = ref(false);
let showDayOptions = ref(false);
let showOptions = ref(false);
let selectedWeek = ref("Все недели");
let selectedDay = ref("Вся неделя");
let weekDays = computed(() =>
  selectedWeek.value === "Все недели"
    ? data.map((w) =>
        w.week === selectedWeek.value ? w.days.map((d) => d.name) : ""
      )
    : Object.values(data)
        .filter((w) => w.week === selectedWeek.value)[0]
        .days.map((d) => d.name)
);
let color = computed(() =>
  selectedWeek.value !== "Все недели" ? "#71717a" : "#d4d4d8"
);

let schedule1 = ref([]);
let schedule2 = ref([]);
function filt() {
  // schedule.value.push(
  //   selectedWeek.value === "Все недели" ? data : data[selectedWeek.value]
  // );
  // console.log(data, schedule.value)
  // if (selectedDay.value !== "Вся неделя") {
  //   if (selectedWeek.value === "Все недели") {
  //     let result = [];
  //     result.push(
  //       Array(schedule.value[0]["1"]).map((d) => d[selectedDay.value])
  //     );
  //     result.push(
  //       Array(schedule.value[0]["2"]).map((d) => d[selectedDay.value])
  //     );
  //     schedule.value = result;
  //   } else {
  //     schedule.value = schedule.value.map((d) => d[selectedDay.value]);
  //   }
  // } else {
  //   schedule.value = schedule.value[0];
  // }
  schedule1.value = [];
  schedule2.value = [];
  if (selectedWeek.value === "Все недели") {
    schedule1.value = Object.values(data)[0].days;
    schedule2.value = Object.values(data)[1].days;
  } else if (selectedDay.value === "Вся неделя") {
    if (selectedWeek.value === 1) {
      schedule1.value = Object.values(data).filter(
        (w) => w.week === selectedWeek.value
      )[0].days;
    } else {
      schedule2.value = Object.values(data).filter(
        (w) => w.week === selectedWeek.value
      )[0].days;
    }
  } else {
    if (selectedWeek.value === 1) {
      schedule1.value = Object.values(data)
        .filter((w) => w.week === selectedWeek.value)[0]
        .days.filter((d) => d.name === selectedDay.value);
    } else {
      schedule2.value = Object.values(data)
        .filter((w) => w.week === selectedWeek.value)[0]
        .days.filter((d) => d.name === selectedDay.value);
    }
  }
  console.log(schedule1.value);
}
</script>

<template>
  <div class="w-full h-full flex flex-row relative">
    <!-- sidebar -->
    <div class="h-full fixed left-0 top-0 w-[50px] bg-zinc-900"></div>
    <!-- inputs -->
    <div
      class="min-h-[100vh] w-fit absolute top-0 left-[50px] py-[100px] px-[50px] flex flex-col justify-center relative"
    >
      <!-- inputs container -->
      <div class="grid grid-rows-3 gap-[20px] fixed">
        <div class="flex flex-col relative">
          <label>Неделя</label>
          <div
            @click="showWeekOptions = !showWeekOptions"
            class="bg-white rounded w-[300px] p-[8px] h-[50px] cursor-pointer flex items-center"
          >
            <div class="h-min">{{ selectedWeek }}</div>
          </div>
          <div
            v-if="showWeekOptions"
            class="options"
            @input="profile"
            @click="showWeekOptions = !showWeekOptions"
          >
            <div @click="selectedWeek = 'Все недели'" class="option">
              Все недели
            </div>
            <div @click="selectedWeek = 1" class="option">1</div>
            <div @click="selectedWeek = 2" class="option">2</div>
          </div>
        </div>

        <div class="flex flex-col relative">
          <label>День недели</label>
          <div
            @click="
              selectedWeek === 'Все недели'
                ? null
                : (showDayOptions = !showDayOptions)
            "
            class="bg-white rounded w-[300px] p-[8px] h-[50px] cursor-pointer flex items-center"
            :style="{ color: color }"
            value="selectedDay"
          >
            <div class="h-min">{{ selectedDay }}</div>
          </div>
          <div
            v-if="showDayOptions"
            class="options"
            @click="showDayOptions = !showDayOptions"
            @input="profile"
          >
            <div class="option" @click="selectedDay = 'Вся неделя'">
              Вся неделя
            </div>
            <div
              v-for="day of weekDays"
              :key="day"
              @click="selectedDay = day"
              class="option"
            >
              {{ day }}
            </div>
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
        <button
          class="uppercase rounded-full bg-indigo-900 w-fit px-[50px] py-[10px] text-zinc-300 shadow-md hover:opacity-[.8] absolute bottom-0 transition-[.3s] outline-none"
          @click="filt()"
        >
          get
        </button>
      </div>
    </div>

    <!-- schedule -->
    <div class="grid grid-cols-2 relative w-[100%] ml-[350px] mr-[20px]">
      <div class="weekName">Первая</div>
      <div class="week left-0">
        <div class="days" v-for="day of schedule1" :key="day">
          <div class="day">
            <div class="day-name">{{ day.name }}</div>
            <div class="grid grid-rows-6 gap-[10px]">
              <div
                v-for="(subject, index) of day.subjects"
                :key="subject"
                class="subjects"
              >
                <div class="subject-index">{{ index }}.</div>
                <div class="subject" v-if="subject.name">
                  <div class="">
                    <div class="flex flex-row w-full mb-[5px]">
                      <div class="subject-name">{{ subject.name }}</div>
                      <div class="ml-[5px] text-zinc-500">
                        {{ subject.type }}
                      </div>
                      <div
                        v-if="subject.subgroup"
                        class="ml-[10px] text-zinc-500"
                      >
                        {{ subject.subgroup }}п/г
                      </div>
                    </div>
                    <div class="subject-info">
                      <div class="">
                        <span class="text-zinc-500">кабинет: </span
                        >{{ subject.audience }}
                      </div>
                      <div class="">
                        <span class="text-zinc-500">препод: </span
                        >{{ subject.teacher }}
                      </div>
                    </div>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>

      <div class="weekName">Вторая</div>
      <div class="week right-0">
        <div class="days" v-for="day of schedule2" :key="day">
          <div class="day">
            <div class="day-name">{{ day.name }}</div>
            <div class="grid grid-rows-6 gap-[10px]">
              <div
                v-for="(subject, index) of day.subjects"
                :key="subject"
                class="subjects"
              >
                <div class="subject-index">{{ index }}.</div>
                <div class="subject" v-if="subject.name">
                  <div class="">
                    <div class="flex flex-row w-full mb-[5px]">
                      <div class="subject-name">{{ subject.name }}</div>
                      <div class="ml-[5px] text-zinc-500">
                        {{ subject.type }}
                      </div>
                      <div
                        v-if="subject.subgroup"
                        class="ml-[10px] text-zinc-500"
                      >
                        {{ subject.subgroup }}п/г
                      </div>
                    </div>
                    <div class="subject-info">
                      <div class="">
                        <span class="text-zinc-500">кабинет: </span
                        >{{ subject.audience }}
                      </div>
                      <div class="">
                        <span class="text-zinc-500">препод: </span
                        >{{ subject.teacher }}
                      </div>
                    </div>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>