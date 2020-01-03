<template>
  <div class="container p-6">
    <h1 class="text-3xl mb-2">
      Nuxt.js Advent Calendar 2019
    </h1>

    <p class="mb-2">{{ comment }}</p>

    <table class="table-auto w-full border">
      <thead>
        <tr class="flex">
          <th class="border px-4 py-2 day"
            v-for="dayName in dayNames"
            :key="dayName"
          >
            {{ dayName }}
          </th>
        </tr>
      </thead>
      <tbody>
        <tr class="flex"
          v-for="index in [0, 1, 2, 3]"
          :key="index"
        >
          <td class="border px-4 py-2 day"
            v-for="day in days.slice((7 * index), (7 + (7 * index)))"
            :key="day.date"
          >
            <p class="bg-blue-100 text-blue-700 px-2 py-1 font-bold rounded-t">
              {{ day.date }}
            </p>

            <div class="flex justify-between items-center my-2">
              <img
                class="rounded w-1/3"
                :src="day.authorImageUrl"
                :alt="day.authorName"
              />

              <a
                class="block w-3/5 truncate text-blue-500 hover:text-blue-800"
                :href="`https://qiita.com/${day.authorName}`"
                target="_blank"
                rel="noopener noreferrer"
              >
                {{ day.authorName }}
              </a>
            </div>

            <a v-if="day.articleUrl !== null"
              :href="day.articleUrl"
              class="text-blue-500 hover:text-blue-800"
              target="_blank"
              rel="noopener noreferrer"
            >
              {{ day.comment }}
            </a>

            <p v-else
              class="text-gray-900"
            >
              {{ day.comment }}
            </p>
          </td>

          <template v-if="index == 3">
            <td v-for="day in [26, 27, 28]"
              :key="day"
              class="border px-4 py-2 day"
            >
              <p class="bg-gray-100 text-gray-700 px-2 py-1 font-bold rounded-t">
                {{ day }}
              </p>
            </td>
          </template>
        </tr>
      </tbody>
    </table>

    <p class="my-2">
      ※ このページはレスポンシブ対応していません。デスクトップサイズの画面でご覧ください。
    </p>
  </div>
</template>

<script>
import { parse } from 'node-html-parser'
export default {
  async asyncData({ $axios }) {
    const { data } = await $axios.get('https://qiita.com/advent-calendar/2019/nuxt-js')
    const root = parse(data)
    return {
      comment: root.querySelector('.markdownContent').text,
      dayNames: root.querySelectorAll('.adventCalendarCalendar_dayName').map(dayName => dayName.text),
      days: root.querySelectorAll('.adventCalendarCalendar_day').map(day => {
        const articleUrl = day.querySelector('.adventCalendarCalendar_comment').querySelector('a') ?
          day.querySelector('.adventCalendarCalendar_comment').querySelector('a').attributes['href'] :
          null
        return {
          date: day.querySelector('.adventCalendarCalendar_date').text,
          authorName: day.querySelector('.adventCalendarCalendar_author').text.replace(/\s|&nbsp;/g, ''),
          authorImageUrl: day.querySelector('.adventCalendarCalendar_authorIcon').attributes['src'],
          comment: day.querySelector('.adventCalendarCalendar_comment').text,
          articleUrl: articleUrl
        }
      })
    }
  }
}
</script>

<style scoped>
.day {
  display: block;
  width: calc(100% / 7);
}
</style>
