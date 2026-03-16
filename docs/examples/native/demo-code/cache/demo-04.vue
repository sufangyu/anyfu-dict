<script lang="ts" setup>
import { createDictManager } from '@anyfu/dict-core';
import { onMounted, ref } from 'vue';


const maxCacheSizeManager = createDictManager({
  baseURL: 'https://useful-event-b654f7bc61.strapiapp.com',
  url: '/api/dict/base',
  maxCacheSize: 4,
});

const logResult = ref<string[]>([]);
function pintLog(msg: string) {
  logResult.value.push(msg);
  console.log(msg);
}

async function handleValidateMaxCacheSize() {
  // 先塞满 4 个：顺序为 A, B, C, D
  await maxCacheSizeManager.fetchDict('A');
  await maxCacheSizeManager.fetchDict('B');
  await maxCacheSizeManager.fetchDict('C');
  await maxCacheSizeManager.fetchDict('D');
  pintLog(`1.当前缓存的keys:: ${maxCacheSizeManager.getCacheKeys()}`);

  // 再读一次 A（命中缓存）→ A 应移到末尾，顺序变为 B, C, D, A
  await maxCacheSizeManager.fetchDict('A');
  pintLog(`2.当前缓存的keys:: ${maxCacheSizeManager.getCacheKeys()}`);

  // 再请求 E，满容量应淘汰“最久未用”的 B，不是 A
  await maxCacheSizeManager.fetchDict('E');
  pintLog(`3.当前缓存的keys:: ${maxCacheSizeManager.getCacheKeys()}`);
}


onMounted(async () => {
  handleValidateMaxCacheSize();
});
</script>


<template>
  <div>
    <h1 class="text-lg! my-1!">
      结果输入打印:
    </h1>
    <p v-if="logResult.length === 0" class="my-1!">
      ...
    </p>
    <template v-else>
      <p
        v-for="(log, idx) in logResult"
        :key="idx"
        class="my-1!"
      >
        {{ log }}
      </p>
    </template>
  </div>
</template>
