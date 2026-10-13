<template>
  <div class="rank">
    <h4 class="title">{{ title }}</h4>
    <div v-if="loading" class="state">正在加载...</div>
    <div v-else-if="error" class="state error">{{ error }}</div>
    <div v-else-if="!list || list.length === 0" class="state">暂无排行榜数据</div>
    <ul v-else class="list" :style="{ height: height ? `${height}px` : 'auto', overflow: 'auto' }">
      <li :key="index" v-for="(item, index) in list">
        <span :class="index < 3 ? 'active' : null">{{ index + 1 }}</span>
        <span>{{ item.name }}</span>
        <span>{{ item.total }}</span>
      </li>
    </ul>
  </div>
</template>

<script lang="ts">
  import { defineComponent } from 'vue';
  export default defineComponent({
    name: 'RankList',
    props: {
      title: {
        type: String,
        default: '',
      },
      list: {
        type: Array,
        default: null,
      },
      height: {
        type: Number,
        default: null,
      },
      loading: {
        type: Boolean,
        default: false,
      },
      error: {
        type: String,
        default: '',
      },
    },
  });
</script>

<style lang="less" scoped>
  .rank {
    padding: 0 32px 32px 72px;

    .state {
      min-height: 220px;
      display: flex;
      align-items: center;
      justify-content: center;
      color: rgba(0, 0, 0, 0.45);
      font-size: 14px;
    }

    .state.error {
      color: #ff4d4f;
    }

    .list {
      margin: 25px 0 0;
      padding: 0;
      list-style: none;

      li {
        margin-top: 16px;

        span {
          font-size: 14px;
          line-height: 22px;

          &:first-child {
            background-color: #f5f5f5;
            color: rgba(0, 0, 0, 0.65);
            border-radius: 20px;
            display: inline-block;
            font-size: 12px;
            font-weight: 600;
            margin-right: 24px;
            height: 20px;
            line-height: 20px;
            width: 20px;
            text-align: center;
          }
          &.active {
            background-color: #314659;
            color: #fff;
          }
          &:last-child {
            float: right;
          }
        }
      }
    }
  }

  .mobile .rank {
    padding: 0 32px 32px 32px;
  }
</style>
