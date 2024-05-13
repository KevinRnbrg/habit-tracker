<template>
    <div class="bg-black mb-4">
        <v-row class="ma-0">
            <v-col class="d-flex justify-center v-col-1">
                <router-link to="/" class="d-flex text-decoration-none">
                    <!-- <img src="../assets/logo.svg" alt="Logo" width="50"> -->
                    <div class="bg-white rounded-circle">
                        <v-icon size="48">mdi-atom</v-icon>
                    </div>
                    <v-tooltip text="Habits" activator="parent" location="bottom" />
                </router-link>
            </v-col>
            <v-col class="v-col-8 d-flex align-center pa-6 ga-6">
                <router-link to="/" class="navigation-link" text="Habits"></router-link>
                <v-divider class="border-opacity-100" vertical></v-divider>
                <router-link to="/manage" class="navigation-link" text="Manage"></router-link>
            </v-col>
            <v-spacer></v-spacer>
            <v-col class="v-col-2 d-flex flex-column align-center justify-center">
                <span>{{ currentDatetime.day + ', ' + currentDatetime.month + ' ' + currentDatetime.date }}</span>
                <span>{{ currentDatetime.time }}</span>
            </v-col>
        </v-row>
    </div>
</template>
<script setup>
    import { ref, onUnmounted } from 'vue'

    const currentFullDatetime = ref(''); // to be used when tracking habit checkmarks
    const currentDatetime = ref({
        day: '',
        date: '',
        month: '',
        time: ''
    });

    function updateTime() {
        const now = new Date();
        currentFullDatetime.value = now;

        currentDatetime.value.day = now.toLocaleDateString('en-US', {weekday: 'long'});
        currentDatetime.value.date = now.getDate();
        currentDatetime.value.month = now.toLocaleDateString('en-US', { month: 'long' });
        currentDatetime.value.time = now.toLocaleTimeString('en-GB'); 
    }

    updateTime();

    const interval = setInterval(updateTime, 1000);

    onUnmounted(() => clearInterval(interval));
</script>

<style scoped>
    .navigation-link {
        color: white;
        text-decoration: none;
    }

    .navigation-link:hover {
        text-decoration: underline;
    }

</style>