<script lang="ts" setup>
import { createDictManager } from '@anyfu/dict-core';
import { ref } from 'vue';


const retryConfig = ref({
  retry: 2,
  retryDelay: 200,
});

async function handleGetDictRetry() {
  console.log('当前重试配置::', retryConfig.value);
  const retryDictManager = createDictManager({
    baseURL: 'https://useful-event-b654f7bc61.strapiapp.com',
    url: '/api/dict/base-retry',
    retry: 2,
    retryOn: (err) => {
      const status = err?.status;
      return status >= 500 || status === 404 || status === 405;
    },
  });

  await retryDictManager.fetchDict('DICT_RETRY');
}
</script>


<template>
  <div>
    <el-button type="primary" @click="handleGetDictRetry">
      获取字典
    </el-button>
  </div>
</template>
