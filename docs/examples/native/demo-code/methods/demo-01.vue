<script lang="ts" setup>
import type { CacheItem, DictItemDefault } from '@anyfu/dict-core';
import { createDictManager } from '@anyfu/dict-core';
import { onMounted, ref } from 'vue';

const cacheDict = ref<CacheItem<DictItemDefault>>();

const code = 'DICT_FIRST';
const dictManager = createDictManager({
  baseURL: 'https://useful-event-b654f7bc61.strapiapp.com',
  url: '/api/dict/base',
});

onMounted(async () => {
  await dictManager.fetchDict(code);
  cacheDict.value = dictManager.getDictCache(code);
  console.log('字典缓存信息::', cacheDict.value);
});
</script>


<template>
  <div>
    <p>字典缓存信息: {{ cacheDict }}</p>
  </div>
</template>
