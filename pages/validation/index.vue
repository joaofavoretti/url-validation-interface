<template>
  <div>
    <v-card class="ma-5 pa-5">
      <div class="text-h2">Validation</div>
      <div class="text-subtitle-1">This page should allow you to manipulate all the available URLs stored in the database.</div>
      <v-data-table
        items-per-page="10"
        :headers="headers"
        :items="urls"
      >
        <template v-slot:[`item.url`]="{ item }">
          <a :href="item.raw.url">{{ item.raw.url }}</a>
        </template>
        <template v-slot:[`item.validated`]="{ item }">
          <v-chip
            :color="item.raw.validated ? 'green' : 'red'"
            dark
            small
          >
            {{ item.raw.validated ? 'Yes' : 'No' }}
          </v-chip>
        </template>
        <template v-slot:[`item.status`]="{ item }">
          <v-chip
            :color="item.raw.status === 'Online' ? 'green' : 'red'"
            dark
            small
          >
            {{ item.raw.status }}
          </v-chip>
        </template>
        <template v-slot:[`item.id`]="{ item }">
          <!-- Button to details -->
          <v-btn
            icon
            flat
            :to="`/validation/id/${item.raw.id}`"
            small
          >
            <v-icon>mdi-eye</v-icon>
          </v-btn>
        </template>
    </v-data-table>
    </v-card>
  </div>
</template>
<script>
  export default {
  data () {
    return {
      headers: [
        {
          title: 'URL',
          align: 'start',
          sortable: false,
          key: 'url',
        },
        { title: 'Validated', align: 'center', key: 'validated' },
        { title: 'Status', align: 'center', key: 'status' },
        { title: 'Action', align: 'end', key: 'id'}
      ],
      urls: [
        {
          id: 1,
          url: 'https://www.google.com',
          validated: true,
          status: 'Online',
        },
      ],
    }
  },
}
</script>
  