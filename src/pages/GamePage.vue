<template>
  <div id="gamePage">
    <a-row align="space-between">
      <a-button style="margin-bottom: 8px" @click="doBack">back</a-button>
      <a-button>block left: {{ clearBlockNum }} / {{ totalBlockNum }}</a-button>
    </a-row>
    <!-- 胜利 -->
    <a-row align="center">
      <div v-if="gameStatus === 3" style="text-align: center">
        <h2>u win 🎉</h2>
        <!-- <img alt="程序员鱼皮" src="../assets/kunkun.png" /> -->
        <my-ad style="margin-top: 16px" />
      </div>
    </a-row>
    <!-- 分层选块 -->
    <a-row align="center">
      <div v-show="gameStatus > 0" class="level-board">
        <div v-for="(block, idx) in levelBlocksVal" :key="idx">
          <div
            v-if="block.status === 0"
            class="block level-block"
            :class="{
              disabled: !isHolyLight && block.lowerThanBlocks.length > 0,
            }"
            :data-id="block.id"
            :style="{
              zIndex: 100 + block.level,
              left: block.x * widthUnit + 'px',
              top: block.y * heightUnit + 'px',
            }"
            @click="() => doClickBlock(block)"
          >
            <img :src="block.type" :alt="block.type" class="block-img" />
          </div>
        </div>
      </div>
    </a-row>
    <!-- 随机选块 -->
    <a-row align="space-between" class="random-board">
      <div
        v-for="(randomBlock, index) in randomBlocksVal"
        :key="index"
        class="random-area"
      >
        <div
          v-if="randomBlock.length > 0"
          :data-id="randomBlock[0].id"
          class="block"
          @click="() => doClickBlock(randomBlock[0], index)"
        >
          <img
            :src="randomBlock[0].type"
            :alt="randomBlock[0].type"
            class="block-img"
          />
        </div>
        <!-- 隐藏 -->
        <div
          v-for="num in Math.max(randomBlock.length - 1, 0)"
          :key="num"
          class="block disabled"
        >
          <span v-if="canSeeRandom">
            <img
              :src="randomBlock[num].type"
              :alt="randomBlock[num].type"
              class="block-img"
            />
          </span>
        </div>
      </div>
    </a-row>
    <div class="bottom-action">
      <!-- 槽位 -->
      <a-row v-if="slotAreaVal.length > 0" align="center" class="slot-board">
        <div
          v-for="(slotBlock, index) in slotAreaVal"
          :key="index"
          class="block"
        >
          <img
            v-if="slotBlock"
            :src="slotBlock?.type"
            :alt="slotBlock?.type"
            class="block-img"
          />
        </div>
      </a-row>
      <!-- 技能 -->
      <div class="skill-board">
        <a-space>
          <a-button size="small" @click="doRevert">back</a-button>
          <!-- <a-button size="small" @click="doRemove">move out</a-button> -->
          <a-button
            size="small"
            @click="doShuffle"
            :disabled="shuffuleCount >=3"
            >{{ shuffuleCount }}/3 shuffle</a-button
          >
          <!-- <a-button size="small" @click="doBroke">破坏</a-button> -->
          <!-- <a-button size="small" @click="doHolyLight">圣光</a-button> -->
          <!-- <a-button size="small" @click="doSeeRandom">透视</a-button> -->
        </a-space>
      </div>
      <h3>
        Collect 3 similar beras to eliminate. Eliminate all blocks to win !!
      </h3>
    </div>
  </div>
</template>

<script setup lang="ts">
import useGame from "../core/game";
import { onMounted } from "vue";
import { useRouter } from "vue-router";
import MyAd from "../components/MyAd.vue";

const router = useRouter();

const {
  gameStatus,
  levelBlocksVal,
  randomBlocksVal,
  slotAreaVal,
  widthUnit,
  heightUnit,
  totalBlockNum,
  clearBlockNum,
  isHolyLight,
  canSeeRandom,
  doClickBlock,
  doStart,
  doShuffle,
  shuffuleCount,
  doBroke,
  doRemove,
  doRevert,
  doHolyLight,
  doSeeRandom,
} = useGame();

/**
 * 回上一页
 */
const doBack = () => {
  router.back();
};

onMounted(() => {
  doStart();
});
</script>

<style scoped>
.block-img {
  width: 32px;
  height: 32px;
}

.bottom-action {
  position: absolute;
  bottom: 0;
  left: 50%;
  transform: translateX(-50%);
  width: 100%;
  text-align: center;
}
.level-board {
  position: relative;
}

.level-block {
  position: absolute;
}

.random-board {
  margin-top: 8px;
}

.random-area {
  margin-top: 8px;
}

.slot-board {
  border: 10px solid saddlebrown;
  margin: 16px auto;
  width: fit-content;
}

.skill-board {
  text-align: center;
}

.block {
  font-size: 28px;
  width: 42px;
  height: 42px;
  line-height: 42px;
  border: 1px solid #eee;
  border-radius: 4px;
  background: white;
  text-align: center;
  vertical-align: top;
  display: inline-block;
  cursor: pointer;
  transition: all 0.3s;
}

.disabled {
  background: grey;
  opacity: 0.2;
  cursor: not-allowed;
  transition: all 0.3s;
}
</style>
