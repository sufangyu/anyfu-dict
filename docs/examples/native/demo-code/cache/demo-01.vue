<script lang="ts" setup>
import type { DictItemDefault } from '@anyfu/dict-core';
import { createDictManager, getDictItems } from '@anyfu/dict-core';
import { onMounted, ref } from 'vue';


const dictManager = createDictManager({
  baseURL: 'https://useful-event-b654f7bc61.strapiapp.com',
  url: '/api/dict/base',
});


const stylesDictList = ref<DictItemDefault[]>([]);
async function getDictStyles(disableCache = false) {
  const res = await dictManager.fetchDict('DICT_STYLES', { disableCache });
  console.log('DICT_STYLES', res);
  stylesDictList.value = getDictItems(res.DICT_STYLES);
}


onMounted(async () => {
  getDictStyles();
});
</script>


<template>
  <div>
    <p class="my-1!">
      DICT_STYLES: {{ stylesDictList.length }}
    </p>
    <ElButton type="primary" @click="getDictStyles(false)">
      再次获取字典-使用缓存
    </ElButton>
    <ElButton type="danger" @click="getDictStyles(true)">
      再次获取字典-忽略缓存
    </ElButton>
  </div>
</template>
