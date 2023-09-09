<template>
  <div>
    <v-card class="ma-5 pa-5">
      <v-row>
        <v-col class="text-h2">
          Details
        </v-col>
        <v-col cols="auto">
         
          <v-btn
            v-if="!url.manual_inspection"
            dark
            fab
            small
            color="orange"
            class="ml-2"
            @click="url.manual_inspection = { triggered: false, comments: ''}"
          >
            <v-icon>mdi-magnify</v-icon> 
            <span class="ml-2">Inspect</span>
          </v-btn>
          <v-btn
            v-else
            :color="url.manual_inspection.triggered ? 'red' : 'green'"
            dark
            fab
            small
            class="ml-2"
            @click="url.manual_inspection.triggered = !url.manual_inspection.triggered"
          >
            <v-icon>{{ url.manual_inspection.triggered ? 'mdi-virus' : 'mdi-virus-off' }}</v-icon>
            <span class="ml-2">{{ url.manual_inspection.triggered ? 'Malicious' : 'Benign' }}</span>
          </v-btn>
          
          <v-btn
            :color="url.status === 'Online' ? 'blue' : 'grey'"
            dark
            fab
            small
            class="ml-2"
            @click="url.status = url.status === 'Online' ? 'Offline' : 'Online'"
          >
            <v-icon>{{ url.status === 'Online' ? 'mdi-power' : 'mdi-power-off' }}</v-icon>
            <span class="ml-2">{{ url.status }}</span>
          </v-btn>

          <v-btn
            icon
            flat
            small
            class="ml-2"
            @click="assistance = !assistance"
          >
            <v-icon>{{ assistance ? 'mdi-help-circle' : 'mdi-help-circle-outline' }}</v-icon>
          </v-btn>

          <v-btn
            icon
            flat
            small
            class="ml-2"
          >
            <v-icon>mdi-content-save</v-icon>
          </v-btn>
          
          <v-btn
            icon
            flat
            @click="$router.go(-1)"
            small
            class="ml-2"
          >
            <v-icon>mdi-close</v-icon>
        </v-btn>
        </v-col>
      </v-row>
      <v-row>
        <v-col>
          
          <v-list>
            <v-list-item
              title="URL"
            >
              <template v-slot:append>
                <a :href="url.url">{{  url.url }}</a>
              </template>
            </v-list-item>
          </v-list>
    
          <v-expansion-panels>
            <v-expansion-panel
              title="PhishTank Inspection"
              v-if="assistance && url.phishtank_inspection"
            >
              <v-expansion-panel-text>
                <v-row>
                  <v-col>
                    <v-list>
                      <v-list-item
                        title="Triggered"
                      >
                        <template v-slot:append>
                          <v-chip
                            :color="url.phishtank_inspection.triggered ? 'red' : 'green'"
                            dark
                            small
                          >
                            {{ url.phishtank_inspection.triggered ? 'Yes' : 'No' }}
                          </v-chip>
                        </template>
                      </v-list-item>
                    </v-list>
                  </v-col>
                </v-row>
              </v-expansion-panel-text>
            </v-expansion-panel>
            <v-expansion-panel
              title="Google Safe Browsing Inspection"
              v-if="assistance && url.gsb_inspection"
            >
              <v-expansion-panel-text>
                <v-row>
                  <v-col>
                    <v-list>
                      <v-list-item
                        title="Triggered"
                      >
                        <template v-slot:append>
                          <v-chip
                            :color="url.gsb_inspection.triggered ? 'red' : 'green'"
                            dark
                            small
                          >
                            {{ url.gsb_inspection.triggered ? 'Yes' : 'No' }}
                          </v-chip>
                        </template>  
                      </v-list-item>
                      <v-divider />
                      <v-list-item
                        title="Threat Type"
                      >
                        <template v-slot:append>
                          {{ url.gsb_inspection.comments.threatType }}
                        </template>
                      </v-list-item>
                      <v-divider />
                      <v-list-item
                        title="Platform Type"
                      >
                        <template v-slot:append>
                          {{ url.gsb_inspection.comments.platformType }}
                        </template>
                      </v-list-item>
                      <v-divider />
                      <v-list-item
                        title="Threat"
                      >
                        <template v-slot:append>
                          <a :href="url.gsb_inspection.comments.threat.url">{{ url.gsb_inspection.comments.threat.url }}</a>
                        </template>
                      </v-list-item>
                      <v-divider />
                      <v-list-item
                        title="Cache Duration"
                      >
                        <template v-slot:append>
                          {{ url.gsb_inspection.comments.cacheDuration }}
                        </template>
                      </v-list-item>
                      <v-divider />
                      <v-list-item
                        title="Threat Entry Type"
                      >
                        <template v-slot:append>
                          {{ url.gsb_inspection.comments.threatEntryType }}
                        </template>
                      </v-list-item>
                    </v-list>
                  </v-col>
                </v-row>
              </v-expansion-panel-text>
            </v-expansion-panel>
            <v-expansion-panel
              title="Manual Inspection"
              v-if="url.manual_inspection"
            >
              <v-expansion-panel-text>
                <v-row>
                  <v-col>
                    <v-list>
                      <v-list-item
                        title="Triggered"
                      >
                        <template v-slot:append>
                          <v-chip
                            :color="url.manual_inspection.triggered ? 'red' : 'green'"
                            dark
                            small
                          >
                            {{ url.manual_inspection.triggered ? 'Yes' : 'No' }}
                          </v-chip>
                        </template>
                      </v-list-item>
                    </v-list>
                    <v-textarea
                      v-model="url.manual_inspection.comments"
                      label="Comments"
                      outlined
                      rows="5"
                    >
                    </v-textarea>
                  </v-col>
                </v-row>
              </v-expansion-panel-text>
            </v-expansion-panel>
          </v-expansion-panels>
        </v-col>
      </v-row>
    </v-card>
  </div>
</template>
<script>
import api from '~/api/api.js';

export default {
  data () {
    return {
      assistance: false,
      url: {
        id: 1,
        url: 'https://google.com',
        status: 'Online',
        phishtank_inspection: {
          triggered: true,
        },
        gsb_inspection: {
          triggered: true,
          comments: {
            threatType: 'SOCIAL_ENGINEERING',
            platformType: 'ANY_PLATFORM',
            threat: {
              url: 'https://reappeal-ticket-ac9f9.web.app/'
            },
            cacheDuration: '300s',
            threatEntryType: 'URL',
          }
        },
        manual_inspection: null
      }
    }
  },
  async mounted() {
    const res = await api.get('/urls/get')
    console.log(res)
  }
}
</script>
