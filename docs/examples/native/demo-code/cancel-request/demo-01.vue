<script lang="ts" setup>
import { createDictManager } from '@anyfu/dict-core';
import { ElMessage } from 'element-plus';


const dictManager = createDictManager({
  baseURL: 'https://useful-event-b654f7bc61.strapiapp.com',
  url: '/api/dict/base',
  mergeDelay: 125,
});

async function cancelRequest() {
  const p = dictManager.fetchDict(['DICT_CANCEL_REQUEST']);
  // 在合并请求窗口期内取消（请求发出前）
  setTimeout(() => dictManager.cancelFetch(), 50);

  try {
    await p;
  } catch (err) {
    console.log('已取消（请求未发出）', err.message);
    ElMessage.error('已取消（请求未发出）');
  }
}
</script>


<template>
  <div>
    <ElButton type="primary" @click="cancelRequest">
      获取字典
    </ElButton>
  </div>
</template>
