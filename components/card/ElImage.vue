<script lang="ts" setup>
const {
  defaultSrc,
  loadClass,
  previewSrcList,
  errorRootClass,
  errorClass,
  src,
} = defineProps<{
  src?: string
  defaultSrc?: string
  loadClass?: string
  loadRootClass?: string
  errorRootClass?: string
  errorClass?: string
  previewSrcList?: string[]
  ctxName?: string
}>();
const target = computed(() => defaultSrc !== undefined ? BaseUrlImg + defaultSrc : src);

// 打开图片预览
function openPreview() {
  if (previewSrcList && previewSrcList.length > 0) {
    useImageViewer.open({
      urlList: previewSrcList,
      initialIndex: 0,
    });
  }
}
</script>

<template>
  <el-image
    v-if="src !== `${BaseUrlImg}`"
    :src="target"
    fit="cover"
    :draggable="false"
    v-bind="$attrs"
    :preview-teleported="false"
    :preview-src-list="[]"
    :ctx-name="ctxName"
    class="cursor-pointer"
    @click="openPreview"
  >
    <!-- 占位 -->
    <template #placeholder>
      <div :class="loadClass !== undefined ? loadClass : 'sky-loading h-full w-full'" />
    </template>
    <!-- 错误 -->
    <template #error>
      <slot name="error">
        <div :ctx-name="ctxName" class="h-full w-full flex-row-c-c" :class="errorRootClass">
          <i :ctx-name="ctxName" class="icon i-solar-gallery-remove-bold-duotone op-60" :class="errorClass" />
        </div>
      </slot>
    </template>
  </el-image>
  <template v-else>
    <div :ctx-name="ctxName" class="flex-row-c-c text-mini" :class="$attrs.class">
      <i :ctx-name="ctxName" class="icon i-solar-gallery-remove-bold-duotone op-60" :class="errorClass" />
    </div>
  </template>
</template>

<style scoped lang="scss">
.icon {
  --at-apply: "block max-w-4/5 min-h-5 min-w-5 text-gray"
}
</style>
