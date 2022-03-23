<template>
  <div class="flex flex-col">
    <div class="flex flex-row justify-around pb-10">
      <div class="flex flex-col">
        Start date
        <input v-model="startDate" type="date" class="border-gray-300 border-1">
      </div>
      <div class="flex flex-col">
        End date
        <input v-model="endDate" type="date" class="border-gray-300 border-1">
      </div>
    </div>
    <div class="grid grid-cols-7 grid-flow-row">
      <template
        v-for="weekday in weekdayConstants"
        :key="weekday.isoWeekday"
      >
        <div class="flex flex-col justify-items-center">
          <span class="text-gray-700 text-center">{{ weekday.print }}</span>
        </div>
      </template>
      <div
        v-for="day in dateArrayByWeekday"
        :key="day.printDate"
        class="m-4 py-4 px-6"
        :class="[day.display ? 'bg-gray-100' : 'bg-gray-300']"
      >
        <div v-if="day.display">
          <div class="text-center">
            <span>{{ day.printDate }}</span>
          </div>
          <div class="flex flex-col my-2">
            <div class="bg-orange-500 py-2 pl-2 my-1">
              Warning
            </div>
            <div class="bg-red-500 py-2 pl-2 my-1">
              Error
            </div>
            <div class="bg-green-500 py-2 pl-2 my-1">
              Hey all ok 😁
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import moment from 'moment'

const startDate = ref<Date>()
const endDate = ref<Date>()

const weekdayConstants = [
  {
    isoWeekday: 7,
    print: 'SUN',
  },
  {
    isoWeekday: 1,
    print: 'MON',
  },
  {
    isoWeekday: 2,
    print: 'TUE',
  },
  {
    isoWeekday: 3,
    print: 'WED',
  },
  {
    isoWeekday: 4,
    print: 'THU',
  },
  {
    isoWeekday: 5,
    print: 'FRI',
  },
  {
    isoWeekday: 6,
    print: 'SAT',
  },
]

const startMoment = computed(() => moment(startDate.value).startOf('day'))
const endMoment = computed(() => moment(endDate.value).add(1).startOf('day'))

const dateArrayByWeekday = computed(() => {
  const dates: any[] = []

  const iteratorDate = moment(startDate.value).startOf('week')
  const finalDate = moment(endDate.value).add(1, 'day').endOf('week')

  while (!iteratorDate.isSameOrAfter(finalDate)) {
    dates.push({
      day: iteratorDate.day(),
      month: iteratorDate.month(),
      year: iteratorDate.year(),
      isoWeekday: iteratorDate.isoWeekday(),
      printDate: iteratorDate.format('dddd, MMM D YYYY'),
      display: iteratorDate.isBetween(startMoment.value, endMoment.value, 'day', '[]'),
    })
    iteratorDate.add(1, 'day')
  }

  return dates
})

</script>
