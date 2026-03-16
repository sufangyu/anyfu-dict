<script lang="ts" setup>
import type { DictItemDefault } from '@anyfu/dict-core';
import { createDictManager, getDictItems } from '@anyfu/dict-core';
import axios from 'axios';
import { onMounted, ref } from 'vue';


const stylesDictList = ref<DictItemDefault[]>([]);
const reasonDictList = ref<DictItemDefault[]>([]);

const dictManager = createDictManager({
  baseURL: 'https://useful-event-b654f7bc61.strapiapp.com',
  url: '/api/dict/base',
  method: 'post',
  headers: {
    'X-Custom': 'custom-headers',
  },
  requestFn: async (config) => {
    const responseRaw = await axios.post(`${config.url}`, config.data, {
      headers: {
        ...config.headers,
      },
    });

    return responseRaw.data;
  },
});

onMounted(async () => {
  const res = await dictManager.fetchDict(['DICT_STYLES', 'DICT_REASON']);
  stylesDictList.value = getDictItems(res.DICT_STYLES);
  reasonDictList.value = getDictItems(res.DICT_REASON);
});
</script>


<template>
  <div>
    <p class="my-1!">
      DICT_STYLES: {{ stylesDictList.length }}
    </p>
    <p class="my-1!">
      DICT_REASON: {{ reasonDictList.length }}
    </p>
  </div>
</template>
