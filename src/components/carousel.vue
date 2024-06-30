<script setup>
import { reactive, ref } from "vue";

// 轮播图列表
const list = ref(new Array(5).fill(""));
const max = list.value.length - 1;

// 当前显示的下标
const currentIndex = ref(0);

// 文字内容对象
const contentData = ref({});
// 获取文字内容
const getContent = () => {
  fetch("https://v1.jinrishici.com/all.json")
    .then((response) => {
      return response.json();
    })
    .then((data) => {
      contentData.value = data;
    });
};
getContent();

// 前一张按钮点击
const onPreBtnClick = () => {
  if (currentIndex.value <= 0) {
    currentIndex.value = 0;
  } else {
    currentIndex.value--;
    getContent();
  }
};

// 后一张按钮点击
const onNextBtnClick = () => {
  if (currentIndex.value >= max) {
    currentIndex.value = max;
  } else {
    currentIndex.value++;
    getContent();
  }
};
</script>

<template>
  <div class="carousel-container">
    <div
      :class="['pre-button', 'button', currentIndex === 0 ? '' : 'active']"
      @click="onPreBtnClick"
    >
      &lt;
    </div>
    <div class="carousel-wrapper">
      <div class="tag">{{ contentData.category }}</div>
      <div
        v-for="(item, index) in list"
        :key="index"
        class="carousel-item"
        :style="{
          transform: 'translateX(' + (index - currentIndex) * 100 + '%)',
        }"
      >
        <div class="content-box">
          <div class="content">
            {{ contentData.content }}
            <br/>
             <div class="author"> ——《{{ contentData.origin }}》 {{ contentData.author }}
            </div>
          </div>
        </div>
      </div>
      <!-- <div class="author">
        
      </div> -->
    </div>
    <div
      :class="['next-button', 'button', currentIndex >= max ? '' : 'active']"
      @click="onNextBtnClick"
    >
      >
    </div>
  </div>
</template>
<style >
:root {
  --main-color: #ff7963;
  --bg-color: #ff79631f;
  --white-color: #fff;
}

.carousel-container {
  display: flex;
  align-items: center;
  justify-content: center;
}
.button {
  width: 40px;
  height: 40px;
  font-size: 40px;
  border: 2px solid var(--main-color);
  border-radius: 100%;
  color: var(--main-color);
  line-height: 36px;
  opacity: 0.3;
}
.button.active {
  opacity: 1;
  cursor: pointer;
}
.button.active:hover {
  color: var(--white-color);
  border-color: var(--white-color);
  background-color: var(--main-color);
}

.carousel-wrapper {
  width: 600px;
  height: 200px;
  border: 1px solid var(--main-color);
  margin: 0 20px;
  position: relative;
  overflow: hidden;
}
.tag {
  position: absolute;
  left: 6px;
  top: 6px;
  border-left: 4px solid var(--main-color);
  padding: 10px 8px;
  background-color: #ff79632e;
}
.carousel-item {
  width: 100%;
  height: 100%;
  position: absolute;
  left: 0;
  z-index: -1;
  background-color: var(--bg-color);
  transition: all linear 0.5s;
  display: flex;
  align-items: center;
  justify-content: center;
}

.content {
  font-size: 28px;
  font-weight: bold;
  padding: 0 36px;
  /* max-width: 80%;
  text-align: left; */
}
.author {
  font-weight: normal;
  font-size: 16px;
  text-align: right;
  margin-top: 12px;
}
</style>