<script>
import { ref, computed } from 'vue';

export default {
  setup() {
    const weekArray = ref(['Su', 'Mo', 'Tu', 'We', 'Th', 'Fr', 'Sa']);
    const monthArray = ref([
      'Jan',
      'Feb',
      'Mar',
      'Apr',
      'May',
      'Jun',
      'Jul',
      'Aug',
      'Sep',
      'Oct',
      'Nov',
      'Dec',
    ]);
    const date = new Date();
    const year = ref(date.getFullYear());
    const monthIndex = ref(date.getMonth());
    const month = computed(() => monthArray.value[monthIndex.value]);
    const totalDays = computed(() =>
      new Date(year.value, monthIndex.value + 1, 0).getDate()
    );
    const week = computed(() =>
      new Date(year.value, monthIndex.value, 1).getDay()
    );
    const currentDate = computed(() => {
      const y = date.getFullYear();
      const m = date.getMonth();
      return y === year.value && m === monthIndex.value ? date.getDate() : -1;
    });

    const lastMonth = computed(() => {
      const array = [];
      const days = new Date(year.value, monthIndex.value, 0).getDate();
      for (let i = days - week.value; i < days; i += 1) {
        array.push(i);
      }
      return array;
    });

    const nextMonth = computed(() => {
      const array = [];
      let days = 42 - lastMonth.value.length - totalDays.value;
      for (let i = 1; i <= days; i += 1) {
        array.push(i);
      }
      return array;
    });

    function addMonth() {
      monthIndex.value += 1;
      if (monthIndex.value > 11) {
        year.value = year.value + 1;
        monthIndex.value = 0;
      }
    }
    function minutesMonth() {
      monthIndex.value -= 1;
      if (monthIndex.value < 0) {
        year.value = year.value - 1;
        monthIndex.value = 11;
      }
    }
    function refreshMonth() {
      monthIndex.value = date.getMonth();
      year.value = date.getFullYear();
    }

    return {
      weekArray,
      monthArray,

      year,
      month,
      week,
      monthIndex,
      totalDays,
      currentDate,

      lastMonth,
      nextMonth,

      addMonth,
      minutesMonth,
      refreshMonth,
    };
  },
};
</script>
<template>
  <div class="mx-2">
    <div
      class="grid grid-cols-7 p-4 md:w-[500px] w-full border items-center gap-3"
    >
      <h3 class="font-bold col-span-4 text-xl px-3 text-left">
        {{ month }} {{ year }}
      </h3>
      <button class="outline-none text-gray-400 px-3" @click="refreshMonth()">
        <svg
          xmlns="http://www.w3.org/2000/svg"
          class="h-6 w-6"
          fill="none"
          viewBox="0 0 24 24"
          stroke="currentColor"
        >
          <path
            stroke-linecap="round"
            stroke-linejoin="round"
            stroke-width="2"
            d="M4 4v5h.582m15.356 2A8.001 8.001 0 004.582 9m0 0H9m11 11v-5h-.581m0 0a8.003 8.003 0 01-15.357-2m15.357 2H15"
          />
        </svg>
      </button>
      <button
        class="outline-none text-gray-400 px-3"
        type="button"
        @click="addMonth"
      >
        <svg
          xmlns="http://www.w3.org/2000/svg"
          class="h-6 w-6"
          fill="none"
          viewBox="0 0 24 24"
          stroke="currentColor"
        >
          <path
            stroke-linecap="round"
            stroke-linejoin="round"
            stroke-width="2"
            d="M9 11l3-3m0 0l3 3m-3-3v8m0-13a9 9 0 110 18 9 9 0 010-18z"
          />
        </svg>
      </button>
      <button
        class="outline-none text-gray-400 px-3"
        type="button"
        @click="minutesMonth"
      >
        <svg
          xmlns="http://www.w3.org/2000/svg"
          class="h-6 w-6"
          fill="none"
          viewBox="0 0 24 24"
          stroke="currentColor"
        >
          <path
            stroke-linecap="round"
            stroke-linejoin="round"
            stroke-width="2"
            d="M15 13l-3 3m0 0l-3-3m3 3V8m0 13a9 9 0 110-18 9 9 0 010 18z"
          />
        </svg>
      </button>
    </div>
    <div class="border grid grid-cols-7 gap-3 px-4">
      <div
        class="font-bold p-3"
        v-for="week in weekArray"
        :key="week"
        :class="{
          'bg-green-500': week === weekArray[week] && currentDate != -1,
        }"
      >
        {{ week }}
      </div>
    </div>
    <div class="border grid grid-cols-7 p-4 grid-rows-6 gap-3">
      <div
        class="
          font-bold
          p-3
          outline-none
          rounded-full
          w-12
          h-12
          text-gray-200
          relative
        "
        type="button"
        v-for="item in lastMonth"
        :key="item"
      >
        <span
          class="
            text-gray-200 text-sm
            absolute
            -top-2
            left-1/2
            -translate-x-1/2
          "
          :class="{ hidden: item != lastMonth[lastMonth.length - 1] }"
          >{{ monthArray[monthIndex - 1] }}</span
        >
        {{ item }}
      </div>
      <div
        class="font-bold p-3 outline-none rounded-full w-12 h-12 relative"
        :class="[currentDate === item && ['bg-blue-700', 'text-white']]"
        type="button"
        v-for="item in totalDays"
        :key="item"
      >
        <span
          class="text-sm absolute -top-2 left-1/2 -translate-x-1/2"
          :class="{ hidden: item != 1 }"
          >{{ monthArray[monthIndex] }}</span
        >
        {{ item }}
      </div>
      <div
        class="
          font-bold
          p-3
          outline-none
          rounded-full
          w-12
          h-12
          text-gray-200
          relative
        "
        type="button"
        v-for="item in nextMonth"
        :key="item"
      >
        <span
          class="
            text-gray-200 text-sm
            absolute
            -top-2
            left-1/2
            -translate-x-1/2
          "
          :class="{ hidden: item != 1 }"
          >{{ monthArray[monthIndex + 1] }}</span
        >
        {{ item }}
      </div>
    </div>
  </div>
</template>
