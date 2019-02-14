<template>
  <div id="pageDashboard">
    <v-container grid-list-xl fluid>
      <v-layout row wrap>
        <v-flex lg12 sm12 xs12>
          <v-subheader>Scheduling Code Controls</v-subheader>
          <v-layout>
            <v-flex xs3>
              <v-subheader></v-subheader>
            </v-flex>
            <v-flex xs3>
              <v-select
                :items="codes"
                v-model="e1"
                label="Select the Code"
                item-text="code"
                item-value="description"
                single-line
              ></v-select>
            </v-flex>
            <v-flex xs3>
              <v-text-field
                :disabled="true"
                name="input-4"
                label=""
                value=""
                v-model="e1"  
                color="info"
                single-line
              ></v-text-field>
            </v-flex>
          </v-layout>
          <v-tabs icons-and-text  dark color="cyan">
            <v-tabs-slider color="yellow"></v-tabs-slider>
            <v-tab href="#tab-1">
              Operations
              <v-icon>event</v-icon>
            </v-tab>
            <v-tab href="#tab-2">
              Payroll & Billing
              <v-icon>folder_open</v-icon>
            </v-tab>
            <v-tab href="#tab-3">
              Reporting & Display
              <v-icon>account_box</v-icon>
            </v-tab>
            <v-tab-item
              :key="1"
              :id="'tab-' + 1"
            >
              <v-card flat>
                <v-card-text>
                  Operations Contents
                  
                  
                </v-card-text>
              </v-card>
            </v-tab-item>
            <v-tab-item
              :key="2"
              :id="'tab-' + 2"
            >
              <v-card flat>
                <v-card-text>
                  Payroll & Billing Contents

                </v-card-text>
              </v-card>
            </v-tab-item>
            <v-tab-item
              :key="3"
              :id="'tab-' + 3"
            >
              <v-card flat>
                <v-card-text>
                  Reporting & Display Contents

                </v-card-text>
              </v-card>
            </v-tab-item>
          </v-tabs>
          <v-subheader> DataTable Components</v-subheader>
          <v-toolbar flat color="white">
            <v-toolbar-title>Scheduling Control Codes</v-toolbar-title>
            <v-spacer></v-spacer>
            <v-text-field
              v-model="search"
              append-icon="search"
              label="Search"
              single-line
              hide-details
            ></v-text-field>
            <v-dialog v-model="dialog" max-width="700px">
              <v-btn slot="activator" color="primary" dark class="mb-2">New Item</v-btn>
              <v-card>
                <v-card-title>
                  <span class="headline">{{ formTitle }}</span>
                </v-card-title>
                
                <v-card-text>
                  <v-container grid-list-md>
                    <v-layout wrap>
                      <v-flex xs12 sm6 md4>
                        <v-text-field v-model="editedItem.code" label="Code"></v-text-field>
                      </v-flex>
                      <v-flex xs12 sm6 md4>
                        <v-text-field v-model="editedItem.category" label="Category"></v-text-field>
                      </v-flex>
                      <v-flex xs12 sm6 md4>
                        <v-text-field v-model="editedItem.description" label="Description"></v-text-field>
                      </v-flex>
                    </v-layout>
                  </v-container>
                </v-card-text>

                <v-card-actions>
                  <v-spacer></v-spacer>
                  <v-btn color="blue darken-1" flat @click="close">Cancel</v-btn>
                  <v-btn color="blue darken-1" flat @click="save">Save</v-btn>
                </v-card-actions>
              </v-card>
            </v-dialog>
          </v-toolbar>
          <v-data-table
            :headers="headers"
            :items="desserts"
            class="elevation-1"
            :search="search"
          >
            <template slot="items" slot-scope="props">
              <td class="text-xs-center">{{ props.item.code }}</td>
              <td class="text-xs-center">{{ props.item.category }}</td>
              <td class="text-xs-center">{{ props.item.description }}</td>
              <td class="justify-center layout px-0">
                <v-icon
                  small
                  class="mr-2"
                  @click="editItem(props.item)"
                >
                  edit
                </v-icon>
                <v-icon
                  small
                  @click="deleteItem(props.item)"
                >
                  delete
                </v-icon>
              </td>
            </template>
            <template slot="no-data">
              <v-btn color="primary" @click="initialize">Reset</v-btn>
            </template>
          </v-data-table>          
        </v-flex>
      </v-layout>
    </v-container>
  </div>
</template>

<script>
import API from '@/api';
import EChart from '@/components/chart/echart';
import MiniStatistic from '@/components/widgets/statistic/MiniStatistic';
import PostListCard from '@/components/widgets/card/PostListCard';
import ProfileCard from '@/components/widgets/card/ProfileCard';
import PostSingleCard from '@/components/widgets/card/PostSingleCard';
import WeatherCard from '@/components/widgets/card/WeatherCard';
import PlainTable from '@/components/widgets/list/PlainTable';
import PlainTableOrder from '@/components/widgets/list/PlainTableOrder';
import VWidget from '@/components/VWidget';
import Material from 'vuetify/es5/util/colors';
import VCircle from '@/components/circle/VCircle';
import BoxChart from '@/components/widgets/chart/BoxChart';
import ChatWindow from '@/components/chat/ChatWindow';
import CircleStatistic from '@/components/widgets/statistic/CircleStatistic';
import LinearStatistic from '@/components/widgets/statistic/LinearStatistic';
import Codes  from '@/api/code';
export default {
  components: {
    VWidget,
    MiniStatistic,
    ChatWindow,
    VCircle,
    WeatherCard,
    PostSingleCard,
    PostListCard,
    ProfileCard,
    EChart,
    BoxChart,
    CircleStatistic,
    LinearStatistic,
    PlainTable,
    PlainTableOrder    
  },
  data: () => ({
    text: 'Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat.',      
    e1: null,
    codes: Codes,
    color: Material,
    selectedTab: 'tab-1',
    dialog: false,
    headers: [
      {
        text: 'Code',
        align: 'center',
        sortable: false,
        value: 'code'
      },
      { text: 'Category',align: 'center', sortable: false,  value: 'category' },
      { text: 'Description', align: 'center', sortable: false,  value: 'description' },
      { text: 'Actions', value: 'name', align: 'center', sortable: false }
    ],
    desserts: [],
    editedIndex: -1,
    editedItem: {
      code: '',
      category: '',
      description: ''
    },
    defaultItem: {
      code: '',
      category: '',
      description: ''
    },
    search: '',
    linearTrending: [
      {
        subheading: 'Sales',
        headline: '2,55',
        caption: 'increase',
        percent: 15,
        icon: {
          label: 'trending_up',
          color: 'success'
        },
        linear: {
          value: 15,
          color: 'success'
        }
      },
      {
        subheading: 'Revenue',
        headline: '6,553',
        caption: 'increase',
        percent: 10,
        icon: {
          label: 'trending_down',
          color: 'error'
        },
        linear: {
          value: 15,
          color: 'error'
        }
      },
      {
        subheading: 'Orders',
        headline: '5,00',
        caption: 'increase',
        percent: 50,
        icon: {
          label: 'arrow_upward',
          color: 'info'
        },
        linear: {
          value: 50,
          color: 'info'
        }
      }
    ],    
    trending: [
      {
        subheading: 'Email',
        headline: '15+',
        caption: 'email opens',
        percent: 15,
        icon: {
          label: 'email',
          color: 'info'
        },
        linear: {
          value: 15,
          color: 'info'
        }
      },        
      {
        subheading: 'Tasks',
        headline: '90%',
        caption: 'tasks completed.',
        percent: 90,
        icon: {
          label: 'list',
          color: 'primary'
        },
        linear: {
          value: 90,
          color: 'success'
        }
      },        
      {
        subheading: 'Issues',
        headline: '100%',
        caption: 'issues fixed.',
        percent: 100,
        icon: {
          label: 'bug_report',
          color: 'primary'
        },
        linear: {
          value: 100,
          color: 'error'
        }
      },        
    ]    
  }),
  computed: {
    formTitle () {
      return this.editedIndex === -1 ? 'New Item' : 'Edit Code'
    },
    activity () {
      return API.getActivity();
    },
    posts () {
      return API.getPost(3);
    },
    siteTrafficData () {
      return API.getMonthVisit;
    },
    locationData () {
      return API.getLocation;
    }
  },
  watch: {
    dialog (val) {
      val || this.close()
    }
  },
  created () {
    this.initialize()
  },
  methods: {
    initialize () {
      this.desserts = [
        {
          code: '1-1 MEETING WITH',
          category: 'Other',
          description: '1-1 Meeting with edmen'
        },
        {
          code: '1ST',
          category: 'Training',
          description: '1st Aid Training'
        },
        {
          code: 'ACTING UP',
          category: 'Scheduled Shift',
          description: 'ACTING UP'
        },
        {
          code: 'AS',
          category: 'Other',
          description: 'Adjusted Shift'
        },
        {
          code: 'AD',
          category: 'Scheduled Shift',
          description: 'Admin Day'
        },
        {
          code: 'APO',
          category: 'Other',
          description: 'Advanced Pull Out'
        },
        {
          code: 'L',
          category: 'Annual Leave',
          description: 'Annual Leave'
        },
        {
          code: 'BF',
          category: 'Other',
          description: 'Back Fill'
        },
        {
          code: 'B',
          category: 'Other',
          description: 'Berevement Leave'
        },
        {
          code: 'BC',
          category: 'Training',
          description: 'Bowel care'
        }
       ]
    },

    editItem (item) {
      this.editedIndex = this.desserts.indexOf(item)
      this.editedItem = Object.assign({}, item)
      this.dialog = true
    },

    deleteItem (item) {
      const index = this.desserts.indexOf(item)
      confirm('Are you sure you want to delete this code?') && this.desserts.splice(index, 1)
    },

    close () {
      this.dialog = false
      setTimeout(() => {
        this.editedItem = Object.assign({}, this.defaultItem)
        this.editedIndex = -1
      }, 300)
    },

    save () {
      if (this.editedIndex > -1) {
        Object.assign(this.desserts[this.editedIndex], this.editedItem)
      } else {
        this.desserts.push(this.editedItem)
      }
      this.close()
    }
  }

};
</script>
