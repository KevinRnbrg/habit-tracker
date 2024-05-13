<template>
  <div class="d-flex align-center justify-center">
    <template v-if="habitData.length">
      <v-row>
        <v-spacer></v-spacer>
          <template v-if="dailyMaxProgress">
            <habit-card
              title="Daily habits"
              :progress="dailyProgress"
              :max-progress="dailyMaxProgress"
              :habit-data="habitData.filter(obj => obj.occurrence === 'Daily')"
            ></habit-card>
          </template>
          <template v-if="weeklyMaxProgress">
            <habit-card
              title="Weekly habits"
              :progress="weeklyProgress"
              :max-progress="weeklyMaxProgress"
              :habit-data="habitData.filter(obj => obj.occurrence === 'Weekly')"
            ></habit-card>
          </template>
          <template v-if="monthlyMaxProgress">
            <habit-card
              title="Monthly habits"
              :progress="monthlyProgress"
              :max-progress="monthlyMaxProgress"
              :habit-data="habitData.filter(obj => obj.occurrence === 'Monthly')"
            ></habit-card>
          </template>
        <v-spacer></v-spacer>
      </v-row>
    </template>
    <template v-else>
      <v-sheet class="d-flex flex-column align-center justify-center mt-8 pa-4 w-50" :elevation="5">
        <ThreeCube v-if="0"/>
        <ThreeAtom v-else/>
        <span class="mt-6 font-weight-black">You have no habits yet. Navigate to the <router-link to="/manage">Manage</router-link> tab and add new habits!</span>
      </v-sheet>
    </template>
    <!-- floating action button -->
  </div>
</template>

<script setup>
    import { ref, onMounted, watch } from 'vue'
    import { getISOWeek } from 'date-fns';
    import ThreeCube from '@/components/ThreeCube.vue';
    import ThreeAtom from '@/components/ThreeAtom.vue';

    const habitData = ref([]);
    
    const dailyProgress = ref(null);
    const weeklyProgress = ref(null);
    const monthlyProgress = ref(null);

    const dailyMaxProgress = ref(null);
    const weeklyMaxProgress = ref(null);
    const monthlyMaxProgress = ref(null);

    const loadData = () => {
      const savedData = JSON.parse(localStorage.getItem('habits_data'));
      if (savedData) habitData.value = savedData;
      updateHabitChecked();
      calculateProgress();
    }
    
    onMounted(loadData);

    const updateHabitChecked = () => { // add mocha testing
      const currentTime = new Date(); // 2024, 4, 13

      for (let habit of habitData.value) {
        let habitDate = new Date(habit.timestamp);
        switch (habit.occurrence) {
          case 'Daily':
            if (habitDate.toDateString() !== currentTime.toDateString()) {
                habit.checked = false;
            }
            break;
          case 'Weekly':
            if (getISOWeek(habitDate) !== getISOWeek(currentTime)) {
                habit.checked = false;
            }
            break;
          case 'Monthly':
            if (habitDate.getMonth() !== currentTime.getMonth()) {
                habit.checked = false;
            }
            break;
          default:
            break;
        }
      }
    }

    const filterAndCount = (occurrence) => habitData.value.filter(obj => obj.occurrence === occurrence && obj.checked === true).length;

    const calculateProgress = () => {
      dailyMaxProgress.value = habitData.value.filter(obj => obj.occurrence === 'Daily').length;
      weeklyMaxProgress.value = habitData.value.filter(obj => obj.occurrence === 'Weekly').length;
      monthlyMaxProgress.value = habitData.value.filter(obj => obj.occurrence === 'Monthly').length;

      dailyProgress.value = filterAndCount('Daily');
      weeklyProgress.value = filterAndCount('Weekly');
      monthlyProgress.value = filterAndCount('Monthly');
    }

    const saveData = () => {
      localStorage.setItem('habits_data', JSON.stringify(habitData.value));
      calculateProgress();
    }

    watch(habitData, saveData, {deep: true});

</script>

<style scoped>
  #spinning-image {
    animation: spin 15s linear infinite;
  }

  @keyframes spin{
    from {
      transform: rotate(0deg);
    }
    to {
      transform: rotate(360deg);
    }
  }
</style>
