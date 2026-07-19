<template>
  <section class="glass-card showcase-container">
    <div class="showcase-header">
      <div class="showcase-title-group">
        <h2 class="showcase-title">早期教育与亲子启蒙案例库 (30 精选样例)</h2>
        <p class="showcase-subtitle">体验不同月龄的蒙氏感官探索、语言爆发对话、感统大运动与全脑数理逻辑，点击“一键同款生成”即可即刻核算</p>
      </div>
      <div class="showcase-badge">科学育儿 · 免费体验</div>
    </div>

    <!-- 搜索与分类筛选 -->
    <div class="showcase-filter-bar">
      <div class="category-tabs">
        <button 
          v-for="cat in categories" 
          :key="cat"
          class="category-tab"
          :class="{ active: currentCategory === cat }"
          @click="currentCategory = cat"
        >
          {{ cat }}
        </button>
      </div>
      <div class="search-input-wrapper">
        <input 
          v-model="searchQuery"
          type="text"
          placeholder="搜索宝宝月龄、启蒙诉求、亲子游戏、感统训练、流派或关键字..."
          class="search-input"
        />
      </div>
    </div>

    <!-- 样例列表格 Grid -->
    <div class="sample-grid">
      <div 
        v-for="sample in paginatedSamples" 
        :key="sample.id" 
        class="sample-card"
      >
        <div class="sample-card-header">
          <span class="topic-category-tag">{{ sample.category }}</span>
          <span class="style-name-tag">{{ sample.style }}</span>
        </div>
        <div class="sample-original">
          <span class="sample-label">早教目标：</span>“{{ sample.destination }}，需求：{{ sample.topic }}”
        </div>
        <div class="sample-rewritten">
          <span class="sample-label">启蒙方案：</span>{{ sample.core }}
        </div>
        <div class="sample-card-footer">
          <button class="use-sample-btn" @click="$emit('use-sample', sample.topic, sample.destination)">
            一键同款生成
          </button>
        </div>
      </div>
    </div>

    <!-- 空状态提示 -->
    <div v-if="filteredSamples.length === 0" class="empty-showcase">
      未找到匹配的早教启蒙案例，请尝试切换分类或重置搜索关键词。
    </div>

    <!-- 分页组件 -->
    <div v-if="filteredSamples.length > pageSize" class="pagination-bar">
      <button 
        class="page-btn" 
        :disabled="currentPage === 1"
        @click="currentPage--"
      >
        上一页
      </button>
      <span class="page-info">第 {{ currentPage }} / {{ totalPages }} 页 (共 {{ filteredSamples.length }} 条)</span>
      <button 
        class="page-btn" 
        :disabled="currentPage === totalPages"
        @click="currentPage++"
      >
        下一页
      </button>
    </div>
  </section>
</template>

<script setup lang="ts">
import { ref, computed, watch } from 'vue';

defineEmits<{
  (e: 'use-sample', text: string, destination: string): void;
}>();

const categories = ['全部', '语言发音', '大运动体能', '感官统合', '情绪社交', '全脑逻辑'];
const currentCategory = ref('全部');
const searchQuery = ref('');
const currentPage = ref(1);
const pageSize = 6;

interface YoujiaoSample {
  id: number;
  category: string;
  destination: string;
  topic: string;
  style: string;
  core: string;
}

// 精选 30 条早期教育与亲子启蒙案例
const raw30Samples: YoujiaoSample[] = [
  {
    id: 1,
    category: '语言发音',
    destination: '1.5岁 (18个月) 语言发音迟缓与词汇拓展',
    topic: '关注口型模仿、双音节词激发与亲子绘本指认。',
    style: '游戏化亲子互动与语言启蒙流',
    core: '方案：“藏猫猫说名称”互动，使用动物大卡片引导模仿“汪汪/喵喵”，每日固定15分钟亲子绘本指读。'
  },
  {
    id: 2,
    category: '感官统合',
    destination: '2.5岁 (30个月) 专注力训练与手眼协调',
    topic: '关注蒙氏串珠、水米分类与精细动作控制。',
    style: '蒙台梭利感官探索与自理流',
    core: '方案：DIY感官盆（红豆与绿豆分离操作），使用木夹子夹棉花球，提供独立安静操作空间培养专注力。'
  },
  {
    id: 3,
    category: '大运动体能',
    destination: '0.8岁 (10个月) 爬行训练与前庭觉刺激',
    topic: '关注手膝协调爬行、障碍过山谷与平衡诱导。',
    style: '感觉统合与大运动体能流',
    core: '方案：铺设软质枕头隧道与坡道，用发光滚球在前方诱导，通过摇摇马提供前庭觉良性刺激。'
  },
  {
    id: 4,
    category: '情绪社交',
    destination: '3岁 社交分享性格与克服占有欲',
    topic: '关注玩具轮流玩、情绪识别卡片与社交礼仪口诀。',
    style: '情绪管理与社交性格培育流',
    core: '方案：引入沙漏定时器建立“轮流交换规则”，通过“心情小脸谱”识别生气与开心，角色扮演模拟公园交友。'
  },
  {
    id: 5,
    category: '全脑逻辑',
    destination: '2.8岁 形状分类与大小排序启蒙',
    topic: '关注三维空间积木搭建、大中小排序与几何认知。',
    style: '启发式全脑开发与数学思维流',
    core: '方案：“小熊吃饼干”游戏（大熊吃大圆、小熊吃小圆），利用蒙氏插座圆柱体进行手眼空间感感知。'
  },
  {
    id: 6,
    category: '感官统合',
    destination: '1.2岁 (14个月) 触觉敏感与脱敏训练',
    topic: '关注不愿赤脚踩草地、双手怕脏与触觉球按摩。',
    style: '感觉统合与大运动体能流',
    core: '方案：使用不同质地触觉球（刺刺球/软球/羊毛球）全身滚压，设置“彩虹面条感官箱”让双手自由抓握脱敏。'
  },
  {
    id: 7,
    category: '语言发音',
    destination: '2岁 (24个月) 动词使用与完整短句输出',
    topic: '关注“宝宝要吃/爸爸抱抱”双词句拼接与指令理解。',
    style: '游戏化亲子互动与语言启蒙流',
    core: '方案：“小木偶做动作”游戏（跑、跳、飞），教师与家长示范扩张句式“宝宝吃苹果”，给予 positive 反馈。'
  },
  {
    id: 8,
    category: '大运动体能',
    destination: '1.8岁 (22个月) 独脚站立与双脚连续跳跃',
    topic: '关注膝关节发力、跳荷叶游戏与体能素质提升。',
    style: '感觉统合与大运动体能流',
    core: '方案：地面贴彩色圆形胶带作为“小荷叶”，播放《小青蛙跳》儿歌，家长牵双手辅助双脚同时跃起。'
  },
  {
    id: 9,
    category: '情绪社交',
    destination: '2岁 分离焦虑与建立安全依恋',
    topic: '关注入托或父母上班哭闹、告别仪式感与安抚物。',
    style: '情绪管理与社交性格培育流',
    core: '方案：制定固定告别仪式（拥抱+拍手+约定时间），制作“妈妈爱心口袋手帕”，播放《妈妈去上班》绘本。'
  },
  {
    id: 10,
    category: '全脑逻辑',
    destination: '3.5岁 基础数概念与按数取物',
    topic: '关注唱数与点数一致性、1-5实物对应与数量感知。',
    style: '启发式全脑开发与数学思维流',
    core: '方案：“小兔子喂胡萝卜”游戏，出示数字卡片3，引导孩子从篮子里点数拿取3根胡萝卜投进小兔嘴巴。'
  },
  {
    id: 11,
    category: '感官统合',
    destination: '0.5岁 (6个月) 翻身与抬头训练',
    topic: '关注颈部肌肉力量、黑白卡视觉追踪与俯卧抬头。',
    style: '感觉统合与大运动体能流',
    core: '方案：每日安排Tummy Time（俯卧练抬头），在前方30厘米出示高对比度黑白摇铃，左右缓慢移动诱导转头。'
  },
  {
    id: 12,
    category: '语言发音',
    destination: '2.5岁 拟声词扩展与故事讲述能力',
    topic: '关注模仿汽车哔哔、小狗汪汪与故事角色代入。',
    style: '游戏化亲子互动与语言启蒙流',
    core: '方案：搭建“声音动物园”，手偶表演故事《小猪找朋友》，引导孩子发出各种小动物叫声参与剧情发展。'
  },
  {
    id: 13,
    category: '大运动体能',
    destination: '2.2岁 平衡车踩踏与空间方向感',
    topic: '关注双腿交替蹬地、避开障碍物与方向控制。',
    style: '感觉统合与大运动体能流',
    core: '方案：在客厅或小区空地设置圆锥筒障碍赛，引导孩子驾驶无脚踏平衡车完成弯道避让与停靠操作。'
  },
  {
    id: 14,
    category: '情绪社交',
    destination: '1.8岁 发脾气打人与情绪宣泄引导',
    topic: '关注不顺心就扔东西、替代行为引导与情绪降温。',
    style: '情绪管理与社交性格培育流',
    core: '方案：设立“平静小角落”（摆放软垫与捏捏乐），示范“深呼吸吹气球”降温法，用语言替孩子说出感受。'
  },
  {
    id: 15,
    category: '全脑逻辑',
    destination: '3岁 模式识别（AB型颜色规律串珠）',
    topic: '关注红黄红黄规律发现、逻辑推理与动手串珠。',
    style: '启发式全脑开发与数学思维流',
    core: '方案：示范串出“红球-黄球-红球-黄球”彩虹项链，问孩子“接下来该放什么颜色”，培养观察推理能力。'
  },
  {
    id: 16,
    category: '感官统合',
    destination: '1.5岁 听觉辨别与声音定位能力',
    topic: '关注听音辨物、乐器敲击声音区分与听觉专注。',
    style: '蒙台梭利感官探索与自理流',
    core: '方案：制作4种声音沙锤（米粒/红豆/石子/棉花），盲猜敲击声音来源，锻炼听觉分辨与注意力集中。'
  },
  {
    id: 17,
    category: '语言发音',
    destination: '3岁 代词我/你/他与复合句沟通',
    topic: '关注人称代词混淆、表达完整需求与家庭对话。',
    style: '游戏化亲子互动与语言启蒙流',
    core: '方案：“分蛋糕”游戏引导口诀（我吃一块，你吃一块），纠正“宝宝要吃”为“我要吃”，提升社交对话水平。'
  },
  {
    id: 18,
    category: '大运动体能',
    destination: '3岁 双手抛接球与身体协调性',
    topic: '关注预判球落地轨迹、双手抱球与大动作爆发。',
    style: '感觉统合与大运动体能流',
    core: '方案：距孩子1米出示软皮球，示范双手在胸前抱接，渐进增加距离至2米，玩“投篮进大塑料筐”游戏。'
  },
  {
    id: 19,
    category: '情绪社交',
    destination: '2.5岁 学会礼貌说请与谢谢',
    topic: '关注日常礼仪养成、请求帮助与感谢同伴。',
    style: '情绪管理与社交性格培育流',
    core: '方案：家庭“小超市购物”角色扮演，要买东西必须说“请给我”，收到后说“谢谢”，建立文明交往习惯。'
  },
  {
    id: 20,
    category: '全脑逻辑',
    destination: '2.5岁 空间方位词上下里外认知',
    topic: '关注玩具放桌子上/盒子里面等空间感知。',
    style: '启发式全脑开发与数学思维流',
    core: '方案：“寻找藏起来的小熊”游戏，发指令“小熊在椅子下面吗？在箱子里面吗？”，强化空间位置对应。'
  },
  {
    id: 21,
    category: '感官统合',
    destination: '1.8岁 嗅觉与味觉感官分类拓展',
    topic: '关注闻水果香气、尝酸甜苦咸与感官认知。',
    style: '蒙台梭利感官探索与自理流',
    core: '方案：出示柠檬片、香蕉片、苹果片，闭眼蒙眼尝一尝闻一闻，用手卡匹配“酸酸的/甜甜的”口感。'
  },
  {
    id: 22,
    category: '语言发音',
    destination: '1岁 (12个月) 首字发音与开口唤爸爸妈妈',
    topic: '关注单音节发音、亲子面部互动与回应性照顾。',
    style: '游戏化亲子互动与语言启蒙流',
    core: '方案：近距离与宝宝面对面，夸张嘴型示范“爸-爸、妈-妈”，当宝宝发出类似音节时给予拥抱与激赏。'
  },
  {
    id: 23,
    category: '大运动体能',
    destination: '1岁 (12个月) 独站与独立迈出第一步',
    topic: '关注脚掌贴地发力、推学步车与下肢力量。',
    style: '感觉统合与大运动体能流',
    core: '方案：让宝宝背靠沙发独立站立，家长在前方半米伸双手鼓励“走过来”，使用加重学步车辅助迈步。'
  },
  {
    id: 24,
    category: '情绪社交',
    destination: '3.5岁 合作建构与同伴协商能力',
    topic: '关注两人共同搭建城堡、分工合作与沟通。',
    style: '情绪管理与社交性格培育流',
    core: '方案：摆放积木箱，分配任务“你负责搬运长积木，我负责盖屋顶”，体验合作完成宏大搭建的成就感。'
  },
  {
    id: 25,
    category: '全脑逻辑',
    destination: '3岁 影子匹配与图形重叠感知',
    topic: '关注观察物体的轮廓特征、阴影连线与手眼。',
    style: '启发式全脑开发与数学思维流',
    core: '方案：制作苹果、汽车、小狗的黑影剪纸，引导孩子将彩色实物与黑色轮廓准确重叠匹配。'
  },
  {
    id: 26,
    category: '感官统合',
    destination: '2.5岁 独立穿鞋袜与手部自理精细',
    topic: '关注分清左右鞋、拉拉链与剥香蕉皮自理。',
    style: '蒙台梭利感官探索与自理流',
    core: '方案：利用早教忙碌板（Busy Board）进行扣纽扣、拉拉链练习，口诀“鞋头碰碰头”训练独立穿鞋。'
  },
  {
    id: 27,
    category: '语言发音',
    destination: '3.5岁 颠倒歌与儿歌韵律记忆',
    topic: '关注语言节奏感、短时记忆力与儿歌背诵。',
    style: '游戏化亲子互动与语言启蒙流',
    core: '方案：教唱传统儿歌《小白兔蹦蹦跳》，配合打拍拍手动作，引导孩子接龙补全最后两个字韵脚。'
  },
  {
    id: 28,
    category: '大运动体能',
    destination: '2.8岁 双脚连续跳绳前置体能准备',
    topic: '关注原地弹跳、双手摇摆与节奏协调。',
    style: '感觉统合与大运动体能流',
    core: '方案：地面放置短绳让孩子双脚跳过，双手持彩带做划圈摇摆动作，锻炼协调与弹跳力量。'
  },
  {
    id: 29,
    category: '情绪社交',
    destination: '1.2岁 陌生人焦虑与社会性微笑',
    topic: '关注见生人躲藏、安全依恋与渐进式接触。',
    style: '情绪管理与社交性格培育流',
    core: '方案：抱着孩子在安全距离外观察亲友，允许孩子搂紧家长，待孩子放松后再尝试挥手打招呼。'
  },
  {
    id: 30,
    category: '全脑逻辑',
    destination: '3.5岁 简单因果关系与生活常识',
    topic: '关注因为天下雨所以带伞、因为饿了所以吃饭。',
    style: '启发式全脑开发与数学思维流',
    core: '方案：出示卡片对对碰（雨云与雨伞、天黑与睡床），引导孩子用“因为……所以……”完整表达逻辑因果。'
  }
];

const samples = ref<YoujiaoSample[]>(raw30Samples);

const filteredSamples = computed(() => {
  return samples.value.filter(s => {
    const matchCat = currentCategory.value === '全部' || s.category === currentCategory.value;
    const matchQuery = !searchQuery.value.trim() || 
      s.topic.includes(searchQuery.value) || 
      s.destination.includes(searchQuery.value) ||
      s.style.includes(searchQuery.value) || 
      s.core.includes(searchQuery.value);
    return matchCat && matchQuery;
  });
});

const totalPages = computed(() => Math.ceil(filteredSamples.value.length / pageSize) || 1);

const paginatedSamples = computed(() => {
  const start = (currentPage.value - 1) * pageSize;
  return filteredSamples.value.slice(start, start + pageSize);
});

watch([currentCategory, searchQuery], () => {
  currentPage.value = 1;
});
</script>
