<script lang="ts" setup>
import ToDoList from './components/ToDoList.vue'
import { ref, onMounted, } from 'vue';
import { createClient } from '@supabase/supabase-js'


const supabase = createClient('https://qmsjnqzrbfyhezshwbhi.supabase.co', 'eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJzdXBhYmFzZSIsInJlZiI6InFtc2pucXpyYmZ5aGV6c2h3YmhpIiwicm9sZSI6ImFub24iLCJpYXQiOjE3MzEyNzM4ODIsImV4cCI6MjA0Njg0OTg4Mn0.HUBeZE6nKik3qGtVd4LjuwxBQtv-hnne42NCIy2xdgU')
interface Item {
  id: number;
  title: string;
  details: string;
  status: boolean;
}

const data = ref<{ items: Item[] }>({
  items: []
});
const activeItems = ref<Item[]>([]);
const completedItems = ref<Item[]>([]);

async function fetchData() {
  data.value.items.splice(0)
  const { data: items} = await supabase
    .from('app-data')
    .select('*')
  data.value.items = items ?? []
  activeItems.value.splice(0)
  completedItems.value.splice(0)
  for (let i = 0; i < data.value.items.length; i++) {
    if (data.value.items[i].status === true) {
      activeItems.value.push({
        id: data.value.items[i].id,
        title: data.value.items[i].title,
        details: data.value.items[i].details,
        status: data.value.items[i].status
      });
    }
    else {
      completedItems.value.push(data.value.items[i]);
    }

  }
}  
onMounted(() => {
  fetchData()
})

const newItemTitle = ref('');
const newItemDetails = ref('');

const removeItem = async (id: number) => {
  const item = data.value.items.find(item => item.id === parseInt(id.toString()));
  if (item) {
    const {} = await supabase
      .from('app-data')
      .delete()
      .eq('id', item.id);

    fetchData()
};
};
const changeState = async (id: number) => {
  const item = data.value.items.find(item => item.id === parseInt(id.toString()));
  if (item) {
    const {} = await supabase
      .from('app-data')
      .update({ status: !item.status })
      .eq('id', item.id);
    fetchData()
  }
};  

const addItem = async () => {
  if (newItemTitle.value && newItemDetails.value) {
    const {} = await supabase
      .from('app-data')
      .insert([{ id: Math.round(Math.random()*1000000), title: newItemTitle.value, details: newItemDetails.value, status: true }]);
    newItemTitle.value = '';
    newItemDetails.value = '';
    fetchData()
  }
  else {
    alert('Please fill in both fields');
    newItemTitle.value = '';
    newItemDetails.value = '';
  }
};
</script>

<template>
    <div>
      <v-text-field v-model="newItemTitle" placeholder="Title" />
      <v-text-field v-model="newItemDetails" placeholder="Details" />
      <v-btn @click="addItem">Add Item</v-btn>
    </div>
  <div class="boxes">
    <ToDoList :state="true" :items="activeItems" @remove-item="removeItem" @toggle-done="changeState"/>
    <ToDoList :state="false" :items="completedItems" @remove-item="removeItem" @toggle-done="changeState"/>
  </div>
  
</template>
<style>
.boxes
{
  display: flex;
  justify-content: center;
  align-items: center;
  width: 100vw;
  min-height: 100vh;
}
</style>


