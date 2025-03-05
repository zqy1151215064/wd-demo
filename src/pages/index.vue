<!--
 * @Author: weisheng
 * @Date: 2024-11-01 11:44:38
 * @LastEditTime: 2024-11-01 14:10:20
 * @LastEditors: weisheng
 * @Description:
 * @FilePath: \wot-demo\src\pages\index.vue
 * 记得注释
-->
<script setup lang="ts">
import { useToast } from 'wot-design-uni'

const tempMin = ref(18)
const tempMax = ref<number | string>(35)
const toast = useToast()

function changeMaxTemp({ value }: { value: number | string }) {
  const num = Number(value)
  if (num < tempMin.value) {
    tempMax.value = 0
    toast.show('温度上限不能小于温度下限')
  }
}

function beforeChange(value: number | string) {
  const num = Number(value)
  if (num >= tempMin.value) {
    return true
  }
  return false
}
</script>

<template>
  <view>
    <wd-cell title="温度下限">
      <wd-input-number v-model="tempMin" input-width="64px" :min="15" :max="Number(tempMax)" :precision="0" />
    </wd-cell>
    <wd-cell title="温度上限">
      <wd-input-number
        v-model="tempMax" allow-null input-width="64px" :min="tempMin" :max="40" :precision="0"
        :before-change="beforeChange" @blur="changeMaxTemp"
      />
    </wd-cell>
  </view>
</template>

<style></style>

<route type="home" lang="json">
{
  "layout": "tabbar",
  "name": "home",
  "style": {
    "navigationBarTitleText": "home"
  }
}
</route>
