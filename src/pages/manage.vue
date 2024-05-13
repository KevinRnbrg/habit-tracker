<template>
    <v-sheet class="d-flex flex-column align-center pa-2">
        <v-card class="w-50" variant="outlined">
            <template v-slot:title>
                <span class="font-weight-black text-h5">Add a new habit</span>
            </template>
            <v-form 
                @submit.prevent="handleFormSubmit" 
                class="d-flex flex-column pa-4"
            >
                <v-row>
                    <v-col class="v-col-8"> <!-- stop the user writing on 60 char -->
                        <v-text-field 
                            v-model="habitName"
                            label="Habit name"
                            density="comfortable"
                            variant="outlined"
                            :rules="rules"
                            required
                            counter="60"
                        ></v-text-field>
                    </v-col>
                    <v-col>
                        <v-select
                            v-model="habitOccurrence"
                            label="Select occurrence"
                            density="comfortable" 
                            variant="outlined"
                            :items="['Daily', 'Weekly', 'Monthly']"
                        ></v-select>
                    </v-col>
                </v-row>
                <v-row>
                    <v-spacer></v-spacer>
                    <v-col class="d-flex justify-center">
                        <v-btn 
                            variant="outlined"
                            type="submit"
                            text="Add"
                        ></v-btn>
                    </v-col>
                    <v-spacer></v-spacer>
                </v-row>
            </v-form>
        </v-card>
        <v-card class="w-50 mt-6" variant="outlined">
            <v-data-table 
                :headers="headers"
                :items="habits"
                density="comfortable"
            >
                <template v-slot:item="{ item }">
                    <tr>
                        <td >
                            <template v-if="!item.editMode">{{ item.name }}</template>
                            <template v-else>
                                <v-text-field
                                    class="my-2"
                                    v-model="item.name"
                                    density="compact" 
                                    variant="outlined"
                                    :rules="rules"
                                    hide-details
                                ></v-text-field>
                            </template>
                        </td>
                        <td>
                            <template v-if="!item.editMode">
                                <v-chip v-if="item.occurrence === 'Daily'" color="yellow-darken-3" density="compact">{{ item.occurrence }}</v-chip>
                                <v-chip v-if="item.occurrence === 'Weekly'" color="pink" density="compact">{{ item.occurrence }}</v-chip>
                                <v-chip v-if="item.occurrence === 'Monthly'" color="orange-darken-4" density="compact">{{ item.occurrence }}</v-chip>
                            </template>
                            <template v-else>
                                <v-select
                                    class="w-75 my-2"
                                    v-model="item.occurrence"
                                    density="compact"
                                    variant="outlined"
                                    :items="['Daily', 'Weekly', 'Monthly']"
                                    hide-details
                                ></v-select>
                            </template>
                        </td>
                        <td class="text-right">
                            <template v-if="!item.editMode">
                                <v-icon @click="toggleEditMode(item)">mdi-pencil</v-icon>
                                <v-icon @click="handleDeleteItem(item)">mdi-delete</v-icon>
                            </template>
                            <template v-else>
                                <v-btn @click="handleEditItem(item)" variant="outlined">Save</v-btn>
                            </template>
                        </td>
                    </tr>
                </template>
            </v-data-table>
        </v-card>

        <v-dialog max-width="500" v-model="dialogActive">
            <template v-slot:activator="{ props: activatorProps }">
                <v-btn
                    class="mt-4"
                    v-bind="activatorProps"
                    color="error"
                    text="Delete all habits"
                    variant="outlined"
                ></v-btn>
            </template>
            <template v-slot:default>
                <v-card title="Confirmation">
                <v-card-text>
                    Are you sure you want to delete all habits?
                </v-card-text>

                <v-card-actions>
                    <v-spacer></v-spacer>
                    <v-btn
                        text="Cancel"
                        @click="dialogActive=false"
                    >
                    </v-btn>
                    <v-btn
                        text="Delete"
                        color="error"
                        @click="handleDeleteAllHabits()"
                    ></v-btn>
                </v-card-actions>
                </v-card>
            </template>
        </v-dialog>
    </v-sheet>
</template>

<script setup>
    import { ref, onMounted } from 'vue';

    const habitName = ref(null);
    const habitOccurrence = ref('Daily');
    const habits = ref([]);

    const dialogActive = ref(false);

    const rules = [
        v => !!v || 'Habit name is required',
        v => (v && v.length <= 60) || 'Max 60 characters'
    ];

    const headers = [
        { title: 'Habit', key: 'name', width: '70%'},
        { title: 'Occurrence', key: 'occurrence', width: '30%'},
        { title: 'Actions', key: '', sortable: false, align: 'end'},
    ]

    const handleFormSubmit = () => {
        if (habitName.value && habitName.value.length <= 60 && habitOccurrence.value) {
            const newHabit = {
                name: habitName.value,
                occurrence: habitOccurrence.value,
                editMode: false, // don't include this in the database
                checked: false,
                timestamp: new Date().toDateString()
            };

            habits.value.push(newHabit);
            saveData();

            habitName.value = null;

        } else {
            // console.error('Please fill all fields'); // display for user
            // Displays same error when too many characters
        }
    };

    const toggleEditMode = (item) => {
        item.editMode = !item.editMode;
    }

    const handleEditItem = (item) => {
        toggleEditMode(item);
        saveData();
    }

    const handleDeleteItem = (item) => {
        const index = habits.value.indexOf(item);
        if (index !== -1) {
            habits.value.splice(index, 1);
            saveData();
        }
    }

    const handleDeleteAllHabits = () => {
        if (habits.value.length > 0) {
            habits.value = [];
            saveData();
        }
        dialogActive.value = false;
    }

    const saveData = () => {
        try {
            localStorage.setItem('habits_data', JSON.stringify(habits.value))
        } catch (e) {
            console.log(e);
        }
    }

    const loadData = () => {
        try {
            const data = JSON.parse(localStorage.getItem('habits_data'));
            if (data) habits.value = data;
        } catch (e) {
            console.log(e);
        }
    }

    onMounted(loadData);
</script>