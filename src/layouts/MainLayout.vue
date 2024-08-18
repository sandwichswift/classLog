<template>
  <q-layout view="lHh Lpr lFf">
    <q-header elevated>
      <q-toolbar>
        <q-btn flat dense round icon="menu" aria-label="Menu" @click="toggleLeftDrawer" />

        <q-toolbar-title>
          1918 Class Log
        </q-toolbar-title>

        <!--用于转换暗黑模式的按钮-->
        <q-btn aria-label="Dark theme toggle" flat round class="tw-ml-4"
          :icon="$q.dark.isActive ? 'brightness_2' : 'brightness_5'" @click="$q.dark.toggle()"></q-btn>
      </q-toolbar>
    </q-header>

    <q-drawer v-model="leftDrawerOpen" show-if-above bordered>
      <q-list>
        <q-item-label header>
          Navigation
        </q-item-label>

        <MdLink v-for="md in mdList" :key="md.author" :author="md.author" :address="md.address" icon="person"
          :changeMd="changeMd" />
      </q-list>
    </q-drawer>

    <q-page-container>
      <MdLayout :address="curAddress" />
    </q-page-container>
  </q-layout>
</template>

<script setup lang="ts">
import { onMounted, ref } from 'vue';
import MdLink, { IMdLink } from 'src/components/MdLink.vue';
import MdLayout from './MdLayout.vue';
defineOptions({
  name: 'MainLayout'
});

let curAddress = ref('src/assets/1.md');
function changeMd(address: string) {
  console.log(address);
  curAddress.value = address;
}

const mdList: IMdLink[] = [];
const baseUrl = 'src/assets/';


async function fetchAuthors() {
  const response = await fetch(baseUrl + 'author.md');
  const text = await response.text();
  const authors = text.split('\n').map(line => line.trim()).filter(line => line);

  for (let i = 0; i < 34; i++) {
    if (i < authors.length) {
      let mdTmp = {
        author: authors[i],
        address: baseUrl + 'logs/' + (i + 1) + '.md',
      }
      mdList.push(mdTmp);
    }
  }
}

onMounted(() => {
  fetchAuthors().then(() => {
    curAddress.value = mdList[14].address;
  });
});


const leftDrawerOpen = ref(false);

function toggleLeftDrawer() {
  leftDrawerOpen.value = !leftDrawerOpen.value;
}
</script>
