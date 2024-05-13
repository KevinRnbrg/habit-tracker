<template>
    <v-col class="v-col-3">
      <v-card :subtitle="subtitle">
        <template v-slot:title>
          <div class="d-flex align-center ga-2">
            <span class="font-weight-black">{{ title }}</span>
            <v-icon v-if="(maxProgress / progress) === 1" size="26">mdi-check-circle</v-icon>
            <v-icon v-else size="26">mdi-check-circle-outline</v-icon>
          </div>
        </template>
        <v-card-text class="bg-surface-light pt-4">
          <v-progress-linear 
            class="mb-2 rounded-pill"
            color="success"
            :height="5"
            :model-value="progress"
            :max="maxProgress"
          ></v-progress-linear>
          <template v-for="(habit) in habitData">
            <div class="d-flex align-center">
              <v-checkbox 
                v-model="habit.checked"
                density="compact"
                hide-details="true"
              >
                <template v-slot:label>{{ habit.name }}</template>
              </v-checkbox>
            </div>
          </template>
        </v-card-text>
      </v-card>
    </v-col>
  </template>
  
  <script>
  export default {
    props: {
      title: String,
      subtitle: String,
      progress: Number,
      maxProgress: Number,
      habitData: Array,
    },
  };
  </script>
  