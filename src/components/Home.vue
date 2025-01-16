<script setup>
import { useTransactionsStore } from '@/stores/transactions';
import { ref, computed } from 'vue';
import { useDateFormat, useNow } from '@vueuse/core';

const transactionsStore = useTransactionsStore();

const dialog = ref(false);
const page = ref(1);
const search = ref('');

const transactions = computed(() => {
    return transactionsStore.transactions
})
</script>

<template>
    <v-app>
        <v-data-iterator v-if="transactions.length" :items="transactions" :items-per-page="5" :search="search">
            <template v-slot:header>
                <v-toolbar class="px-2">
                    <v-text-field v-model="search" density="comfortable" placeholder="Search"
                        prepend-inner-icon="mdi-magnify" style="max-width: 300px;" variant="solo" clearable
                        hide-details></v-text-field>
                </v-toolbar>
            </template>

            <template v-slot:default="{ items }">
                <v-container class="pa-2" fluid>
                    <v-row dense>
                        <v-col v-for="item in items" :key="item.id" cols="auto" md="4">
                            <v-card class="pb-3" border flat>
                                <v-list-item :subtitle="item.raw.dateAdded" class="mb-2">
                                    <template v-slot:title>
                                        <strong class="text-h6 mb-2">{{ item.raw.name }}</strong>
                                    </template>
                                </v-list-item>

                                <div class="d-flex justify-space-between px-4">
                                    <div class="d-flex align-center text-caption text-medium-emphasis me-1">
                                        <v-icon icon="mdi-clock" start></v-icon>

                                        <div class="text-truncate">{{ item.raw.amount }} $</div>
                                    </div>

                                    <v-btn class="text-none" size="small" text="Read" border flat>
                                    </v-btn>
                                </div>
                            </v-card>
                        </v-col>
                    </v-row>
                </v-container>
            </template>

            <template v-slot:footer="{ page, pageCount, prevPage, nextPage }">
                <div class="d-flex align-center justify-center pa-4">
                    <v-btn :disabled="page === 1" density="comfortable" icon="mdi-arrow-left" variant="tonal" rounded
                        @click="prevPage"></v-btn>

                    <div class="mx-2 text-caption">
                        Page {{ page }} of {{ pageCount }}
                    </div>

                    <v-btn :disabled="page >= pageCount" density="comfortable" icon="mdi-arrow-right" variant="tonal"
                        rounded @click="nextPage"></v-btn>
                </div>
            </template>
        </v-data-iterator>
        <p v-else>
            No Transactions Found. Click on the plus sign to add a new one
        </p>
        <v-btn position="fixed" location="bottom right" class="ma-4" size="large" icon="mdi-plus" to="/add-transaction" />
    </v-app>
</template>



