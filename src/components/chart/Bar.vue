<template>
  <div ref="chartRef" :style="{ height, width }"></div>
</template>
<script lang="ts">
  import { defineComponent, PropType, ref, Ref, reactive, watch, onMounted, nextTick } from 'vue';
  import { useECharts } from '/@/hooks/web/useECharts';
  import { cloneDeep } from 'lodash-es';
  export default defineComponent({
    name: 'bar',
    props: {
      chartData: {
        type: Array,
        default: () => [],
      },
      option: {
        type: Object,
        default: () => ({}),
      },
      width: {
        type: String as PropType<string>,
        default: '100%',
      },
      height: {
        type: String as PropType<string>,
        default: 'calc(100vh - 78px)',
      },
      // update-begin--author:liaozhiyang---date:20240407---for：【QQYUN-8762】首页默认及echars颜色调整
      seriesColor: {
        type: String,
        default: '#1890ff',
      },
      // update-end--author:liaozhiyang---date:20240407---for：【QQYUN-8762】首页默认及echars颜色调整
    },
    setup(props) {
      const chartRef = ref<HTMLDivElement | null>(null);
      const { setOptions, resize } = useECharts(chartRef as Ref<HTMLDivElement>);
      const option = reactive({
        tooltip: {
          trigger: 'axis',
          axisPointer: {
            type: 'shadow',
            label: {
              show: true,
              backgroundColor: '#333',
            },
          },
        },
        xAxis: {
          type: 'category',
          data: [],
        },
        yAxis: {
          type: 'value',
        },
        series: [
          {
            name: 'bar',
            type: 'bar',
            data: [],
            color: props.seriesColor,
          },
        ],
      });

      // 旧实现使用 watchEffect，在 Tab 初次隐藏时可能拿到高度为 0 的容器，
      // 导致 ECharts 只绘制坐标轴，切换回来后也不会自动恢复。
      // watchEffect(() => {
      //   props.chartData && initCharts();
      // });

      watch(
        () => props.chartData,
        () => {
          initCharts();
        },
        { deep: true, immediate: true },
      );

      onMounted(() => {
        nextTick(() => {
          initCharts();
        });
      });

      function initCharts() {
        if (props.option) {
          Object.assign(option, cloneDeep(props.option));
        }
        let seriesData = props.chartData.map((item) => {
          return item.value;
        });
        let xAxisData = props.chartData.map((item) => {
          return item.name;
        });
        option.series[0].data = seriesData;
        // update-begin--author:liaozhiyang---date:20240407---for：【QQYUN-8762】首页默认及echars颜色调整
        option.series[0].color = props.seriesColor;
        // update-end--author:liaozhiyang---date:20240407---for：【QQYUN-8762】首页默认及echars颜色调整
        option.xAxis.data = xAxisData;
        nextTick(() => {
          setOptions(option);
          // 等待 Tab/Card 完成布局后重新计算图表尺寸。
          window.requestAnimationFrame(() => resize());
        });
      }
      return { chartRef };
    },
  });
</script>
