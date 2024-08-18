/**
* Copyright © 2024 Dashen. All rights reserved.
*
* @author: Dashen
* @date: 2024-08-15
*/
<template>
  <!-- 插入解析为html的md文档 -->
  <div class="md-body" v-html="compiledMarkdown"></div>
</template>

<script lang="ts" setup>
import { ref, computed, onUpdated } from 'vue';
import { marked } from 'marked';
import hljs from 'highlight.js';
import 'highlight.js/styles/github.css'; // 选择一个样式


const markdownContent = ref('');

const fetchMarkdown = async () => {// 从本地文件获取 markdown 内容
  const response = await fetch(address.address);
  markdownContent.value = await response.text();
};

const address = defineProps<{// 传入 markdown 文件地址,限制string类型
  address: string;
}>();

const compiledMarkdown = computed(() => {
  const renderer = new marked.Renderer();

  renderer.code = ({ text, lang }) => {
    const validLang = lang && hljs.getLanguage(lang) ? lang : 'plaintext';
    const highlighted = hljs.highlight(validLang, text).value;
    return `<pre><code class="hljs ${validLang}">${highlighted}</code></pre>`;
  };

  return marked(markdownContent.value, { renderer });
});

onUpdated(() => {// 监听更新
  fetchMarkdown();
});
</script>

<style>
@import 'github-markdown-css';

.md-body {
  box-sizing: border-box;
  min-width: 200px;
  max-width: 980px;
  margin: 0 auto;
  padding: 45px;
}

@media screen and (max-width: 768px) {
  .md-body {
    padding: 15px;
  }
}

.md-body h1 {
  margin-top: 0;
  margin-bottom: 0.5em;
  font-size: 3em;
}

.md-body h2 {
  margin-top: 1.5em;
  margin-bottom: 0.5em;
  font-size: 2.5em;
}

.md-body h3 {
  margin-top: 1em;
  margin-bottom: 0.5em;
  font-size: 2em;
}

.md-body h4 {
  margin-top: 1em;
  margin-bottom: 0.5em;
  font-size: 1.5em;
}

.md-body>p,
li {
  margin-top: 1em;
  margin-bottom: 1em;
  font-size: 1.35em;
  font-family: '微软雅黑', monospace, sans-serif;
}

.md-body>p>strong {
  font-size: 1.5em;
  font-family: '微软雅黑', monospace, sans-serif;
}
</style>
