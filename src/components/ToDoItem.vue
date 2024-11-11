<script lang="ts" setup>


const props = defineProps({
  id: Number,
  title: String,
  details: String,
  status: Boolean

  
});
</script>

<template>
    <div>
      <v-card-title>{{ props.title }}</v-card-title>
      <v-card-text>{{ props.details }}</v-card-text>
    </div>
    <div>
      <v-btn @click="$emit('toggle-done', props.id)">
        <v-icon :icon="props.status ? 'mdi-checkbox-marked' : 'mdi-checkbox-blank-outline'"></v-icon>
      </v-btn>
      <v-btn @click="$emit('remove', props.id)">
        <v-icon icon="mdi-trash-can"></v-icon>
      </v-btn>
    </div>
</template>

<style scoped>
/* Your styles here */
</style>