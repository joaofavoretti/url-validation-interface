<template>
  <div>
    <v-card class="ma-5 pa-5">
      <v-row>
        <v-col class="text-h2">
          Statistics
        </v-col>
      </v-row>
      <v-row>
        <v-col>
          <v-list>
            <v-list-item
              title="Validated"
            >
              <template v-slot:append>
                {{ amountValidated.amount }} / {{ amountValidated.total }} ({{ (amountValidated.amount * 100 / amountValidated.total).toFixed(1) }}%)
              </template>
            </v-list-item>

            <v-list-item
              title="Online"
            >
              <template v-slot:append>
                {{ amountOnline.onlineAmount }} / {{ amountOnline.total }} ({{ (amountOnline.onlineAmount * 100 / amountOnline.total).toFixed(1) }}%)
              </template>
            </v-list-item>

            <v-list-item
              title="Confidence"
            >
              <template v-slot:append>
                {{ confidence.found }} / {{ confidence.total }} ({{ (confidence.found * 100 / confidence.total).toFixed(1) }}%)
              </template>
            </v-list-item>
          </v-list>
        </v-col>
      </v-row>
    </v-card>
  </div>
</template>

<script setup>
import StatisticsService from '~/api/statisticsService';

const { data: amountValidated } = await StatisticsService.getAmountValidated();
const { data: amountOnline } = await StatisticsService.getAmountOnline();
const { data: confidence } = await StatisticsService.getConfidence();

</script>
