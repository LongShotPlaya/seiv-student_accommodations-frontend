<script setup>
import { ref, onMounted } from "vue";
import AccomCatServices from "../services/accCatServices";
import { useRouter } from "vue-router";
import { VDataTable } from "vuetify/labs/VDataTable"

const router = useRouter();
const message = ref("");

const accCats = ref([]);
const selAccCat = ref([]);

const refreshAccCats = () => {
    AccomCatServices.getAllAccomodationCats()
        .then((response) => {
            accCats.value = response.data;
        })
        .catch((err) => {
            message.value = err.response.data.message
        });
};

const deleteAccCat = () => {
    AccomCatServices.deleteAccomodationCat(selAccCat.value[0].id)
        .then((response) => {
            selAccCat.value = [];
            refreshAccCats();
        })
        .catch((err) => {
            message.value = err.response.data.message;
        });
};

onMounted(() => {
    refreshAccCats();
});
</script>

<template>
    <v-container>
        <v-card>
            <v-card-title class="text-center">Accommodation Categories</v-card-title>
            <v-data-table
                v-model="selAccCat"
                :headers="[
                    { title: 'Name', align: 'start', key: 'name'},
                    { title: 'Description', align: 'start', key: 'description' },
                    { title: 'Email(s)', align: 'start', key: 'email' }
                ]"
                :items="accCats"
                :sort-by="[{ key: 'name', order: 'asc' }]"
                :items-per-page="10"
                return-object
                select-strategy='single'
                show-select
            ></v-data-table>
            <br>
            <v-row justify="center">
                <v-col cols="1">
                    <v-btn
                        color="secondary"
                        @click="router.push({ name: 'accommodationCategory', params: { id: 'add' } })"
                    >Add</v-btn>
                </v-col>
                <v-col cols="1">
                    <v-btn
                        :color="(selAccCat.length != 1) ? `` : `secondary`"
                        :disabled="selAccCat.length != 1"
                        @click="router.push({ name: 'accommodationCategory', params: { id: selAccCat[0].id }})"
                    >Edit</v-btn>
                </v-col>
                <v-col cols="1">
                    <v-btn
                        color="primary" 
                        :disabled="selAccCat.length <= 0"
                        @click="deleteAccCat" 
                    >Delete</v-btn>
                </v-col>
            </v-row>
            <br>
        </v-card>
    </v-container>
</template>

<style>

</style>