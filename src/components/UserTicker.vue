<template>
  <div class="glass-card ticker-card">
    <div class="ticker-wrapper">
      <div v-for="(item, index) in tickerItems" :key="index" class="ticker-item">
        <span>{{ item.text }}</span>
        <span style="color: var(--success-color); font-weight: bold;">[生成完成]</span>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref } from 'vue';

const tickerItems = ref([
  {
    "text": "用户 138****2211 获得了 2 岁宝宝撕纸粘贴感统游戏方案"
  },
  {
    "text": "用户 189****9900 完成了 4 岁室内客厅障碍赛跑体能设计"
  },
  {
    "text": "用户 131****4455 生成了幼小衔接图形分类与专注力打卡"
  },
  {
    "text": "用户 156****7788 取得了小超人角色扮演语言启蒙台词"
  },
  {
    "text": "用户 173****3311 获得了蒙氏早教亲子互动家长鼓励说辞"
  }
]);
</script>
