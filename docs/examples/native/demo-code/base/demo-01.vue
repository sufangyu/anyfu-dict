<script lang="ts" setup>
import type { DictItemDefault } from '@anyfu/dict-core';
import { createDictManager, getDictItems } from '@anyfu/dict-core';
import { onMounted, ref } from 'vue';

const dictData = ref<Record<string, DictItemDefault>>({});

const code = 'DICT_FIRST';
const dictManager = createDictManager({
  baseURL: 'https://useful-event-b654f7bc61.strapiapp.com',
  url: '/api/dict/base',
});

onMounted(async () => {
  const res = await dictManager.fetchDict(code);
  dictData.value = res;
  console.log('字典查询结果::', res, getDictItems(res[code]));
});
</script>


<template>
  <div>
    <p>字典数据: {{ dictData }}</p>
  </div>
</template>
