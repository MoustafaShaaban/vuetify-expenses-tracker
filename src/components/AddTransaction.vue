<template>
    <v-row align="center" justify="center" dense>
        <v-col cols="8" md="6">
            <v-card class="mx-auto" title="Add Transaction">
                <template v-slot:append>
                    <v-btn-toggle v-model="transactionValue" color="primary" rounded="0">
                        <v-btn>
                            Expense
                        </v-btn>

                        <v-btn value="income">
                            Income
                        </v-btn>
                    </v-btn-toggle>
                    <v-avatar size="24">
                        <v-img alt="John" src="https://cdn.vuetifyjs.com/images/john.png"></v-img>
                    </v-avatar>
                </template>
                <v-form v-model="valid" @submit.prevent="handleSubmit">

                    <v-col>
                        <v-text-field v-model="transactionName" :counter="10" label="Transaction Name"
                            required></v-text-field>
                        <v-text-field type="number" v-model="transactionAmount" :counter="10" label="Transaction Amount"
                            required></v-text-field>

                        <v-select clearable chips label="Tags" v-model="tags" :items="transactionsTags" multiple
                            variant="solo" item-title="name">
                            <template v-slot:item="{ props, item }">
                                <v-list-item v-bind="props" :subtitle="item.raw.name"></v-list-item>
                            </template>
                        </v-select>
                    </v-col>

                    <v-btn type="submit" position="fixed" color="blue" location="bottom right" class="ma-4" size="large"
                        icon="mdi-check" />
                </v-form>
            </v-card>
        </v-col>
    </v-row>
    <v-btn position="fixed" color="red" location="bottom left" class="ma-4" size="large" icon="mdi-arrow-left
" to="/" />
</template>

<script setup>
import { ref, watch } from 'vue';
import { useRouter } from "vue-router";
import { nanoid } from 'nanoid';
import { useTransactionsStore } from '../stores/transactions';


const transactionsStore = useTransactionsStore();

const transactionName = ref("");
const transactionAmount = ref("");
const transactionType = ref('');
const transactionDate = ref(Date.now())

const transactionValue = ref(false)

const tags = ref([]);
const transactionsTags = ref([]);

transactionsTags.value = transactionsStore.tags;

// const options = ref(transactionsTags.value)

const router = useRouter();

// function filterFunction(val, update, abort) {
//     update(() => {
//         const needle = val.toLowerCase()
//         options.value = tags.filter(v => v.toLowerCase().indexOf(needle) > -1)
//     })
// }

watch(transactionValue, () => {
    if (transactionValue.value) {
        transactionType.value = 'Income'
        transactionAmount.value = Math.abs(transactionAmount.value)
    } else {
        transactionType.value = 'Expense'
        transactionAmount.value = -Math.abs(transactionAmount.value)
    }
})

function handleSubmit() {
    try {

        if (transactionValue.value) {
            transactionType.value = 'Income'
        } else {
            transactionType.value = 'Expense'
            transactionAmount.value = -Math.abs(transactionAmount.value)
        }

        transactionsStore.addTransaction({
            id: nanoid(),
            name: transactionName.value,
            amount: transactionAmount.value,
            transactionValue: transactionValue.value,
            tags: tags.value,
            type: transactionType.value,
            dateAdded: transactionDate.value || Date.now(),
        })

        router.push('/');

        // Notify.create({
        //     message: 'Transaction Added Successfully',
        //     type: "positive",
        //     actions: [
        //         { icon: 'close', color: 'white', round: true, }
        //     ]
        // })

    } catch (error) {

        //     Notify.create({
        //         message: error.message,
        //         type: "negative",
        //         actions: [
        //             { icon: 'close', color: 'white', round: true, }
        //         ]
        //     })
        // }
    }
}
</script>