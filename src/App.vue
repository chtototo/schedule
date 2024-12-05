<script setup>
import { RouterLink, RouterView } from "vue-router";
import { ref, computed } from "vue";
import { data } from "./data.js";

let showWeekOptions = ref(false);
let showDayOptions = ref(false);
let showOptions = ref(false);
let selectedWeek = ref("Все недели");
let selectedDay = ref("Вся неделя");
let scheduleData = Object.values(data);
let weekDays = [
  "Понедельник",
  "Вторник",
  "Среда",
  "Четверг",
  "Пятница",
  "Суббота",
];

let schedule1 = ref([]);
let schedule2 = ref([]);
function filt() {
  schedule1.value = "";
  schedule2.value = "";
  let schedule_1 = getFullWeek(1).getDay(selectedDay.value);
  let schedule_2 = getFullWeek(2).getDay(selectedDay.value);
  if (selectedWeek.value === 1) {
    schedule1.value = schedule_1;
  } else if (selectedWeek.value === 2) {
    schedule2.value = schedule_2;
  } else {
    schedule1.value = schedule_1;
    schedule2.value = schedule_2;
  }
}

function getFullWeek(week) {
  return scheduleData.filter((w) => w.week === week)[0].days;
}

Array.prototype.getDay = function (day) {
  if (day === "Вся неделя") {
    return this;
  }
  return this.filter((d) => d.name === day);
};

function borderColor(type) {
  let blue = "#60a5fa";
  let indigo = "#818cf8";
  let emerald = "#64748b";
  switch (type) {
    case "лек":
      return blue;
    case "лаб":
      return emerald;
    case "пр":
      return indigo;
    default:
      return indigo;
  }
}

console.log(0.1 + 0.2);
</script>

<template>
  <!-- sidebar -->
  <div class="sidebar"></div>
  <main>
    <!-- inputs container -->
    <div class="inputs-container">
      <div class="input-container">
        <label>Неделя</label>
        <div @click="showWeekOptions = !showWeekOptions" class="input">
          <div class="h-min">{{ selectedWeek }}</div>
        </div>
        <div
          v-if="showWeekOptions"
          class="options"
          @click="showWeekOptions = !showWeekOptions"
        >
          <div @click="selectedWeek = 'Все недели'" class="option">
            Все недели
          </div>
          <div @click="selectedWeek = 1" class="option">1</div>
          <div @click="selectedWeek = 2" class="option">2</div>
        </div>
      </div>

      <div class="input-container">
        <label>День недели</label>
        <div
          @click="showDayOptions = !showDayOptions"
          class="input"
          :style="{ color: color }"
          value="selectedDay"
          @input="filt()"
        >
          <div class="h-min">{{ selectedDay }}</div>
        </div>
        <div
          v-if="showDayOptions"
          class="options"
          @click="showDayOptions = !showDayOptions"
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
      <button class="btn" @click="filt()">get</button>
    </div>

    <!-- schedule -->
    <div class="schedule-container">
      <div class="week">
        <div class="weekName">Первая</div>
        <div class="weekdays">
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
                  <div
                    class="subject"
                    v-if="subject.name"
                    :style="{ 'border-color': borderColor(subject.type) }"
                  >
                    <div class="">
                      <div class="flex flex-row w-full mb-[5px] justify-between">
                        <div class="flex flex-row">
                          <div class="subject-name">{{ subject.name }}</div>
                          <div class="ml-[5px] text-zinc-500">
                            {{ subject.type }}
                          </div>
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
      <div class="week">
        <div class="weekName">Вторая</div>
        <div class="weekdays">
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
                  <div
                    class="subject"
                    v-if="subject.name"
                    :style="{ 'border-color': borderColor(subject.type) }"
                  >
                    <div class="">
                      <div class="flex flex-row w-full mb-[5px] justify-between">
                        <div class="flex flex-row">
                          <div class="subject-name">{{ subject.name }}</div>
                          <div class="ml-[5px] text-zinc-500">
                            {{ subject.type }}
                          </div>
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
  </main>
</template>