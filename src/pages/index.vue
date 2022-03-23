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
        :key="weekday.weekday"
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
import * as Moment from 'moment'
import { extendMoment } from 'moment-range'

const moment = extendMoment(Moment)

const startDate = ref<Date>()
const endDate = ref<Date>()

const weekdayConstants = moment.weekdaysShort(true).map((weekdayText, index) => ({
  weekday: index,
  print: weekdayText,
}))

const startMoment = computed(() => moment(startDate.value).startOf('day'))
const endMoment = computed(() => moment(endDate.value).add(1).startOf('day'))

const dateArrayByWeekday = computed(() => {
  const dates: any[] = []

  const iteratorDate = moment(startDate.value).startOf('week')
  const finalDate = moment(endDate.value).endOf('week')

  const range = moment.range(iteratorDate, finalDate)

  for (const day of range.by('day')) {
    dates.push({
      day: day.day(),
      month: day.month(),
      year: day.year(),
      weekday: day.weekday(),
      printDate: day.format('ddd, MMM D YYYY'),
      display: day.isBetween(startMoment.value, endMoment.value, 'day', '[]'),
    })
  }

  return dates
})

</script>
