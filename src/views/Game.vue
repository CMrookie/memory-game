<template>
  <section class="memory-game">
    <div class="memory-card"
      :class="{ flip: cardsData[index].isFlip }"
      :style="{ order: cardsData[index].order }"
      v-on:click="flipCard(index)"
      v-for="(card, index) in cardsData"
      :key="index">
      <img class="front-face" src="@/assets/images/js-badge.svg" alt="Memory Card">
      <img class="back-face" :src="card.src" :alt="card.framework">
    </div>
  </section>
</template>

<script>
import react from '@/assets/images/react.svg';
import angular from '@/assets/images/angular.svg'
import ember from '@/assets/images/ember.svg'
import vue from '@/assets/images/vue.svg'
import backbone from '@/assets/images/backbone.svg'
import aurelia from '@/assets/images/aurelia.svg'
import { setTimeout } from 'timers';
export default {
  name: 'Game',
  data() {
    return {
      firstCard: null,
      secondCard: null,
      hasFlipCard: false,
      lockBoard: false,
      timer: null,
      shuffleList: [],
      images: [react,react,angular,angular,ember,ember,vue,vue,backbone,backbone,aurelia,aurelia],
      cardsData: [/** {index: number, order: number, isFlip: boolean, frameork: string, src: string} */]
    }
  },
  methods: {
    flipCard(index) {
      // 是否给点击事件上锁
      if (this.lockBoard) return;
      // 是否有翻开牌
      if (!this.hasFlipCard) {
        this.firstCard = index;
        this.cardsData[index].isFlip = true;
        this.hasFlipCard = true;
      } else {
        this.secondCard = index;
        this.cardsData[index].isFlip = true;
        this.lockBoard = true;
        // 检查是否匹配
        this.checkForMatch();
      }
    },
    // 给元素随机位置
    shuffle() {
      // 生成元素的位置
      let order = Math.floor(Math.random() * 12);
      // 判断该位置是否已经分配
      if (this.shuffleList.indexOf(order) === -1) {
        this.shuffleList.push(order);
        return 'order: ' + order;
      } else {
        // 已经分配时重新生成
        this.shuffle()
      }
    },
    checkForMatch() {
      if (this.cardsData[this.firstCard].framework === this.cardsData[this.secondCard].framework) {
        // 两张牌比配正确，重值变量值
        this.resetFlip()
      } else {
        // 匹配不正确，延时对牌进行重置，且重置变量
        this.timer = setTimeout(() => {
          this.cardsData[this.firstCard].isFlip = false;
          this.cardsData[this.secondCard].isFlip = false;
          this.resetFlip();
          this.timer = null
        }, 1500 /* 记忆时间 */)
      }

    },
    // 重置变量，及解锁
    resetFlip() {
      this.firstCard = null;
      this.secondCard = null; 
      this.hasFlipCard = false;
      this.lockBoard = false;
    },
    // 获取用于匹配的值
    getFrameWork(item) {
      let strArr = item.split('/');
      let framework = strArr[strArr.length - 1];

      return framework.split('.')[0];
    },
    // 初始化游戏用的数据
    initData() {
      this.images.forEach((item, index) => {
        this.cardsData.push(
          {
            index: index,
            order: this.shuffleList[index],
            isFlip: false,
            framework: this.getFrameWork(item),
            src: item 
          }
        )
      })
    }
  },
  created() {

    for (let i = 0; i < 12; i++) {
      this.shuffle()
    };

    this.initData()
  }
}
</script>

<style lang="scss">
.memory-game{
  width: 640px;
  height: 640px;
  margin: auto;
  display: flex;
  flex-wrap: wrap;
  perspective: 1000px;
}
.memory-card{
  width: calc(25% - 10px);
  height: calc(33.333% - 10px);
  margin: 5px;
  position: relative;
  box-shadow: 1px 1px 1px rgba(0, 0, 0, .3);
  transform: scale(1);
  transform-style: preserve-3d;
  transition: transform .2s;
  &:active{
    transform: scale(0.97);
    transition: transform .2s;
  }
  &.flip{
    transform: rotateY(180deg);
  }
}
.front-face, .back-face{
  width: 100%;
  height: 100%;
  padding: 20px;
  position: absolute;
  border-radius: 5px;
  background: #1c7ccc;
  backface-visibility: hidden;
}
.back-face{
  transform: rotateY(180deg)
}
</style>
