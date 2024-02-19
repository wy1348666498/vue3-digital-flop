<template>
  {{ num }}
</template>

<script>
export default {
  name: "VueDigitalFlop",
};
</script>

<script setup>
import { ref, watch, nextTick, defineProps } from "vue";
const num = ref(0);
const props = defineProps({
  // 起始值
  start: {
    type: Number,
    default: 0,
  },
  // 目标值
  end: {
    type: Number,
    required: true,
    default: 0,
  },
  // 动画持续时间
  duration: {
    type: Number,
    required: false,
    default: 1000,
  },
  // 分隔符
  separator: {
    type: String,
    required: false,
    default: ",",
  },
  // 小数点
  decimal: {
    type: String,
    default: ".",
  },
  // 前缀
  prefix: {
    type: String,
    default: "",
  },
  // 后缀
  suffix: {
    type: String,
    default: "",
  },
  // 小数点后保留几位
  toFixedNum: {
    type: Number,
    default: 0,
  },
});

// 是否正在进行动画
let isAnimating = false;
let animationFrameId = null;

// 动画函数
function animationTo(duration, form, to, fun) {
  if (form === to) return;
  const speed = (to - form) / duration;
  let num = form;
  const startTime = Date.now();

  // 如果正在进行动画，先停止当前的动画
  if (isAnimating && animationFrameId) {
    cancelAnimationFrame(animationFrameId);
  }
  // 标记为正在进行动画
  isAnimating = true;

  function _run() {
    const now = Date.now();
    const time = now - startTime;
    if (time >= duration) {
      num = to;
      fun && fun(num);
      // 动画结束，标记为不在进行动画
      isAnimating = false;
      return;
    }
    num = form + speed * time;
    fun && fun(num);
    animationFrameId = requestAnimationFrame(_run);
  }
  _run();
}

// 格式化数据，返回想要展示的数据格式
function formatNumber(val) {
  val = val.toFixed(props.toFixedNum);
  val += "";
  const x = val.split(".");
  let x1 = x[0];
  const x2 = x.length > 1 ? props.decimal + x[1] : "";
  const rgx = /(\d+)(\d{3})/;
  if (props.separator && isNaN(parseFloat(props.separator))) {
    while (rgx.test(x1)) {
      x1 = x1.replace(rgx, "$1" + props.separator + "$2");
    }
  }
  return props.prefix + x1 + x2 + props.suffix;
}

watch(
  () => props.end,
  (val, old) => {
    nextTick(() => {
      animationTo(
        props.duration,
        old || props.start || 0,
        val || 0,
        (value) => {
          num.value = formatNumber(value);
        }
      );
    });
  },
  {
    immediate: true,
  }
);
</script>
