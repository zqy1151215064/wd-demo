<!--
 * @Author: weisheng
 * @Date: 2024-11-01 11:44:38
 * @LastEditTime: 2024-11-14 19:02:48
 * @LastEditors: weisheng
 * @Description:
 * @FilePath: \wot-demo\src\pages\hi.vue
 * 记得注释
-->
<script lang="ts" setup>
import dayjs from 'dayjs'

const calendarRef = ref()
const rangeDate = ref<number[]>([dayjs().subtract(6, 'day').valueOf(),
dayjs().valueOf()])
const shortcuts = ref<Record<string, any>[]>([
  { text: '近15天', id: 15 },
  { text: '近30天', id: 30 },
  { text: '近45天', id: 45 },
  { text: '近90天', id: 90 },
])
const formatter = (day) => {
  const date = dayjs(day.date) // 使用 dayjs 解析日期
  const now = dayjs() // 当前时间

  // 判断是否是今天
  if (date.isSame(now, 'day')) {
    day.topInfo = '今天'
  }

  // 设置开始、结束、开始/结束的底部信息
  if (day.type === 'start') {
    day.bottomInfo = '开始'
  }

  if (day.type === 'end') {
    day.bottomInfo = '结束'
  }

  if (day.type === 'same') {
    day.bottomInfo = '开始/结束'
  }

  // 禁用今天之后的日期
  if (date.isAfter(now, 'day')) {
    day.disabled = true // 禁用该日期
  }

  return day
}

const onShortcutsClick = ({ item }) => {
  const dayDiff = item.id - 1
  const endDate = dayjs()
  const startDate = endDate.subtract(dayDiff, 'day')

  return [startDate.valueOf(), endDate.valueOf()]
}

const handleConfirm = async ({ value }) => {
  console.log(value,)
  calendarRef.value?.close()
}
const openCalendar = () => {
  calendarRef.value?.open()
}

</script>

<template>
  <view class="px-3 py-20 text-center hi">
    <view class="text-2xl text-gray-9!">
      <wd-text custom-class="text-gray-9!" text="欢迎使用 Wot Design Uni！" />
    </view>
    <view class="bg-[#F9F9F9] border-rd-lg min-w-42 py-2 px-4 flex items-center gap-2" @click="openCalendar">
      <wd-icon name="time" size="14px" color="#47CCA2" />
      <view class="text-[#2F384C] text-6 font-500">
        {{
          `${dayjs(rangeDate[0]).format('YYYY-MM-DD')} - ${dayjs(rangeDate[1]).format('YYYY-MM-DD')}`
        }}
      </view>
    </view>

    <wd-calendar ref="calendarRef" v-model="rangeDate" label="日期选择" :shortcuts="shortcuts"
      :on-shortcuts-click="onShortcutsClick" :max-range="90" :min-date="dayjs().subtract(89, 'day').valueOf()"
      type="daterange" :formatter="formatter" @confirm="handleConfirm" />
  </view>
</template>

<style lang="scss" scoped>
.hi :deep(.wd-popup) {
  bottom: 50px;
}
</style>

<route lang="json">{
  "layout": "tabbar",
  "name": "hi",
  "style": {
    "navigationBarTitleText": "hi"
  }
}</route>
