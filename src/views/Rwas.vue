<template>
  <div id="pageDashboard">
    <v-container grid-list-xl fluid>
      <v-layout row wrap>
        <v-flex lg12 sm12 xs12>
            <v-subheader>Remote Attendance Welfare System (RAWS)</v-subheader>
            <v-subheader> RWA03 </v-subheader>
            <v-layout>
            </v-layout>
            <v-toolbar flat color="white" style="height: 140px;">
                <v-layout column class="justify-center">
                    <v-btn  color="primary" dark class="mb-2">222</v-btn>
                    <div style="text-align:center">
                        <v-btn  color="blue" icon dark><v-icon>cached</v-icon></v-btn>
                    </div>
                </v-layout>
                <v-layout>
                    <SortedTable :values="values">
                        <thead>
                            <tr>
                                <th scope="col" style="text-align: center; width: 10rem;">
                                    
                                </th>
                                <th scope="col" style="text-align: center; width: 15rem;">
                                    
                                </th>
                                <th scope="col" style="text-align: center; width: 10rem;">
                                    Mins Before Start
                                </th>
                                <th scope="col" style="text-align: center; width: 10rem;">
                                    Mins after End
                                </th>
                            </tr>
                        </thead>
                        <tbody slot="body" slot-scope="sort">
                            <tr v-for="value in sort.values" :key="value.id">
                                <td style="text-align: center;">{{ value.id }}</td>
                                <td style="text-align: center;">{{ value.name }}</td>
                                <td style="text-align: center;">{{ value.start }}</td>
                                <td style="text-align: center;">{{ value.end }}</td>
                            </tr>
                        </tbody>
                    </SortedTable>
                </v-layout>
                <v-btn color="primary" dark class="mb-2">ColdStart</v-btn>
                <v-btn  color="primary" dark class="mb-2">Sign-In</v-btn>
                <v-btn  color="primary" dark class="mb-2">Sign-Off</v-btn>
                <v-btn  color="primary" dark class="mb-2">Welfare</v-btn>
                <v-btn  color="primary" dark class="mb-2">Memo</v-btn>
                <v-flex xs1 sm1 d-flex >
                    <v-select
                    :items="frequencies"
                    label="2"
                    solo
                    ></v-select>
                </v-flex>
                <v-spacer></v-spacer>
            
            </v-toolbar>
            <v-data-table
                :headers="headers1"
                :items="desserts1"
                class="elevation-1"
                :search="search"
            >
                <template slot="items" slot-scope="props">
                    <td class="text-xs-center">{{ props.item.id }}</td>
                    <td class="text-xs-center">{{ props.item.date }}</td>
                    <td class="text-xs-center">{{ props.item.nextdue }}</td>
                    <td class="text-xs-center" style="background-color:cyan">{{ props.item.nextevent }}</td>
                    <td class="text-xs-center">{{ props.item.employee }}</td>
                    <td class="text-xs-center">{{ props.item.contact }}</td>
                    <td class="text-xs-center">{{ props.item.location }}</td>
                    <td class="text-xs-center">{{ props.item.shift }}</td>
                    <td class="text-xs-center"  style="background-color:red">{{ props.item.status }}</td>
                    <td class="text-xs-center">{{ props.item.nextrdq }}</td>
                </template>
                <template slot="no-data">
                <v-btn color="primary" @click="initialize">Reset</v-btn>
                </template>
            </v-data-table>
            <v-subheader> Audit logs </v-subheader>
            <v-layout>
            </v-layout>
            <v-toolbar flat color="white">
                <v-btn  color="blue" icon dark><v-icon>cached</v-icon></v-btn>
                <v-btn color="primary" dark class="mb-2">Audit Log</v-btn>
                <v-btn  color="primary" dark class="mb-2">Clear Audit Log</v-btn>
                <v-btn  color="primary" dark class="mb-2">Clear Yest</v-btn>
                <v-btn  color="primary" dark class="mb-2">Clear Today</v-btn>
                <v-btn  color="primary" dark class="mb-2">Clear Tomorrow</v-btn>
                <v-btn  color="primary" dark class="mb-2">Purge>24</v-btn>
                <v-spacer></v-spacer>
            
            </v-toolbar>
            <v-data-table
                :headers="headers2"
                :items="desserts2"
                class="elevation-1"
                :search="search"
            >
                <template slot="items" slot-scope="props">
                    <td class="text-xs-center">{{ props.item.id }}</td>
                    <td :style="'background-color:' + (props.item.color==0?'none':'yellow')" class="text-xs-center" >{{ props.item.qddate }}</td>
                    <td class="text-xs-center">{{ props.item.qdtime }}</td>
                    <td class="text-xs-center">{{ props.item.nextevent }}</td>
                    <td :style="'background-color:' + (props.item.color==0?'none':'yellow')" class="text-xs-center">{{ props.item.employee }}</td>
                    <td class="text-xs-center">{{ props.item.contact }}</td>
                    <td :style="'background-color:' + (props.item.color==0?'none':'yellow')" class="text-xs-center">{{ props.item.location }}</td>
                    <td :style="'background-color:' + (props.item.color==0?'none':'yellow')" class="text-xs-center">{{ props.item.shiftdate }}</td>
                    <td :style="'background-color:' + (props.item.color==0?'none':'yellow')" lass="text-xs-center">{{ props.item.sftime }}</td>
                    <td :style="'background-color:' + (props.item.color==0?'none':'yellow')" class="text-xs-center">{{ props.item.status }}</td>
                    <td class="text-xs-center">{{ props.item.rtcid }}</td>
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
    codes: [],
    color: Material,
    selectedTab: 'tab-1',
    dialog: false,
    headers1: [
      {
        text: 'No',
        align: 'center',
        sortable: true,
        value: 'id'
      },
      { text: 'Date',align: 'center', sortable: false,  value: 'date'},
      { text: 'Next Due', align: 'center', sortable: false,  value: 'nextdue'},
      { text: 'Next Event', align: 'center', sortable: false,  value: 'nextevent'},
      { text: 'Employee', align: 'center', sortable: false,  value: 'employee'},
      { text: 'Contact', align: 'center', sortable: false,  value: 'contact'},
      { text: 'Location', align: 'center', sortable: false,  value: 'location'},
      { text: 'Shift', align: 'center', sortable: false,  value: 'shift'},
      { text: 'Status', align: 'center', sortable: false,  value: 'status'},
      { text: 'NextRDQ', align: 'center', sortable: false,  value: 'nextrdq'}
    ],
    headers2: [
      {
        text: 'No',
        align: 'center',
        sortable: true,
        value: 'id'
      },
      { text: 'QD Date',align: 'center', sortable: false,  value: 'qddate'},
      { text: 'QD Time Due', align: 'center', sortable: false,  value: 'qdtime'},
      { text: 'Next Event', align: 'center', sortable: false,  value: 'nextevent'},
      { text: 'Employee', align: 'center', sortable: false,  value: 'employee'},
      { text: 'Contact', align: 'center', sortable: false,  value: 'contact'},
      { text: 'Location', align: 'center', sortable: false,  value: 'location'},
      { text: 'Shift Date', align: 'center', sortable: false,  value: 'shiftdate'},
      { text: 'S/F Time', align: 'center', sortable: false,  value: 'sftime'},
      { text: 'Status', align: 'center', sortable: false,  value: 'status'},
      { text: 'RTCID', align: 'center', sortable: false,  value: 'rtcid'}
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
    items: ['Training', 'Scheduled Shift', 'Annual Leave', 'Other'],
    categories: ['Annual Leave','Leave Without pay','Long Service Leave','Meeting','No Show','Other','Other(nRDO)','RDO','Scheduled Shift','Sick','Sleepover','Training','Workers Comp'],
    frequencies:['1','2','3','4','5'],
    values: [
        { name: "Show shifts due for sign-on", id: 2, start: 60, end: 1 },
        { name: "COLDSTART call employees", id: 1, start: 15, end: 1 },
        { name: "Show shifts due for sign-off", id: 3, start: 15, end: 1 }
    ],
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
      return this.editedIndex === -1 ? 'New Code' : 'Edit Code'
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
      this.desserts1 = [
        {
          id: '1',
          date: '22/04/2018',
          nextdue: '06:00',
          nextevent: 'Sign-In',
          employee: '[SP190] FA',
          contact: '0423',
          location: 'Pmp Clayton',
          shift: '06:00-18:00',
          status: 'OverDue',
          nextrdq: '22/04/2018-06:00,24/04/2018-06:00'
        },
        {
          id: '2',
          date: '22/04/2018',
          nextdue: '06:00',
          nextevent: 'Sign-In',
          employee: '[SP190] FA',
          contact: '0423',
          location: 'Pmp Clayton',
          shift: '06:00-18:00',
          status: 'OverDue',
          nextrdq: '22/04/2018-06:00,24/04/2018-06:00'
        },
        {
          id: '3',
          date: '22/04/2018',
          nextdue: '06:00',
          nextevent: 'Sign-In',
          employee: '[SP190] FA',
          contact: '0423',
          location: 'Pmp Clayton',
          shift: '06:00-18:00',
          status: 'OverDue',
          nextrdq: '22/04/2018-06:00,24/04/2018-06:00'
        },
        {
          id: '4',
          date: '22/04/2018',
          nextdue: '06:00',
          nextevent: 'Sign-In',
          employee: '[SP190] FA',
          contact: '0423',
          location: 'Pmp Clayton',
          shift: '06:00-18:00',
          status: 'OverDue',
          nextrdq: '22/04/2018-06:00,24/04/2018-06:00'
        },
        {
          id: '5',
          date: '22/04/2018',
          nextdue: '06:00',
          nextevent: 'Sign-In',
          employee: '[SP190] FA',
          contact: '0423',
          location: 'Pmp Clayton',
          shift: '06:00-18:00',
          status: 'OverDue',
          nextrdq: '22/04/2018-06:00,24/04/2018-06:00'
        },
        {
          id: '6',
          date: '22/04/2018',
          nextdue: '06:00',
          nextevent: 'Sign-In',
          employee: '[SP190] FA',
          contact: '0423',
          location: 'Pmp Clayton',
          shift: '06:00-18:00',
          status: 'OverDue',
          nextrdq: '22/04/2018-06:00,24/04/2018-06:00'
        },
        {
          id: '7',
          date: '22/04/2018',
          nextdue: '06:00',
          nextevent: 'Sign-In',
          employee: '[SP190] FA',
          contact: '0423',
          location: 'Pmp Clayton',
          shift: '06:00-18:00',
          status: 'OverDue',
          nextrdq: '22/04/2018-06:00,24/04/2018-06:00'
        },
        {
          id: '8',
          date: '22/04/2018',
          nextdue: '06:00',
          nextevent: 'Sign-In',
          employee: '[SP190] FA',
          contact: '0423',
          location: 'Pmp Clayton',
          shift: '06:00-18:00',
          status: 'OverDue',
          nextrdq: '22/04/2018-06:00,24/04/2018-06:00'
        },
        {
          id: '9',
          date: '22/04/2018',
          nextdue: '06:00',
          nextevent: 'Sign-In',
          employee: '[SP190] FA',
          contact: '0423',
          location: 'Pmp Clayton',
          shift: '06:00-18:00',
          status: 'OverDue',
          nextrdq: '22/04/2018-06:00,24/04/2018-06:00'
        },
        {
          id: '10',
          date: '22/04/2018',
          nextdue: '06:00',
          nextevent: 'Sign-In',
          employee: '[SP190] FA',
          contact: '0423',
          location: 'Pmp Clayton',
          shift: '06:00-18:00',
          status: 'OverDue',
          nextrdq: '22/04/2018-06:00,24/04/2018-06:00'
        }
       ],
       this.desserts2 = [
        {
          id: '1',
          qddate: '22/04/2018',
          qdtime: '06:00',
          nextevent: '10:UNASSN',
          employee: '[SP190] FA',
          contact: '0423',
          location: '18348,21600,Pmp Clayton',
          shiftdate: '25/03/18',
          sftime: '06:00-18:00',
          status:'Due',
          rtcid: '502*CSA04*18347*64800*21600',
          color: 0
        },
        {
          id: '2',
          qddate: '22/04/2018',
          qdtime: '06:00',
          nextevent: '10:UNASSN',
          employee: '[SP190] FA',
          contact: '0423',
          location: '18348,21600,Pmp Clayton',
          shiftdate: '25/03/18',
          sftime: '06:00-18:00',
          status:'Due',
          rtcid: '502*CSA04*18347*64800*21600',
          color: 1
        },
        {
          id: '3',
          qddate: '22/04/2018',
          qdtime: '06:00',
          nextevent: '10:UNASSN',
          employee: '[SP190] FA',
          contact: '0423',
          location: '18348,21600,Pmp Clayton',
          shiftdate: '25/03/18',
          sftime: '06:00-18:00',
          status:'Due',
          rtcid: '502*CSA04*18347*64800*21600',
          color: 0
        },
        {
          id: '4',
          qddate: '22/04/2018',
          qdtime: '06:00',
          nextevent: '10:UNASSN',
          employee: '[SP190] FA',
          contact: '0423',
          location: '18348,21600,Pmp Clayton',
          shiftdate: '25/03/18',
          sftime: '06:00-18:00',
          status:'Due',
          rtcid: '502*CSA04*18347*64800*21600',
          color: 1
        },
        {
          id: '5',
          qddate: '22/04/2018',
          qdtime: '06:00',
          nextevent: '10:UNASSN',
          employee: '[SP190] FA',
          contact: '0423',
          location: '18348,21600,Pmp Clayton',
          shiftdate: '25/03/18',
          sftime: '06:00-18:00',
          status:'Due',
          rtcid: '502*CSA04*18347*64800*21600',
          color: 1
        },
        {
          id: '6',
          qddate: '22/04/2018',
          qdtime: '06:00',
          nextevent: '10:UNASSN',
          employee: '[SP190] FA',
          contact: '0423',
          location: '18348,21600,Pmp Clayton',
          shiftdate: '25/03/18',
          sftime: '06:00-18:00',
          status:'Due',
          rtcid: '502*CSA04*18347*64800*21600',
          color: 0
        },
        {
          id: '7',
          qddate: '22/04/2018',
          qdtime: '06:00',
          nextevent: '10:UNASSN',
          employee: '[SP190] FA',
          contact: '0423',
          location: '18348,21600,Pmp Clayton',
          shiftdate: '25/03/18',
          sftime: '06:00-18:00',
          status:'Due',
          rtcid: '502*CSA04*18347*64800*21600',
          color: 1
        },
        {
          id: '8',
          qddate: '22/04/2018',
          qdtime: '06:00',
          nextevent: '10:UNASSN',
          employee: '[SP190] FA',
          contact: '0423',
          location: '18348,21600,Pmp Clayton',
          shiftdate: '25/03/18',
          sftime: '06:00-18:00',
          status:'Due',
          rtcid: '502*CSA04*18347*64800*21600',
          color: 0
        },
        {
          id: '9',
          qddate: '22/04/2018',
          qdtime: '06:00',
          nextevent: '10:UNASSN',
          employee: '[SP190] FA',
          contact: '0423',
          location: '18348,21600,Pmp Clayton',
          shiftdate: '25/03/18',
          sftime: '06:00-18:00',
          status:'Due',
          rtcid: '502*CSA04*18347*64800*21600',
          color: 1
        },
        {
          id: '10',
          qddate: '22/04/2018',
          qdtime: '06:00',
          nextevent: '10:UNASSN',
          employee: '[SP190] FA',
          contact: '0423',
          location: '18348,21600,Pmp Clayton',
          shiftdate: '25/03/18',
          sftime: '06:00-18:00',
          status:'Due',
          rtcid: '502*CSA04*18347*64800*21600',
          color: 0
        },
        {
          id: '11',
          qddate: '22/04/2018',
          qdtime: '06:00',
          nextevent: '10:UNASSN',
          employee: '[SP190] FA',
          contact: '0423',
          location: '18348,21600,Pmp Clayton',
          shiftdate: '25/03/18',
          sftime: '06:00-18:00',
          status:'Due',
          rtcid: '502*CSA04*18347*64800*21600',
          color: 0
        }
        ],
       this.codes=this.desserts
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
<style scoped>
  .tab1{
    height: 60px;
  }
  .tab1-special{
    padding-top:0px!important;
   
  }
  .tab2{
    height:50px;
  }
  .tab3{
    height:60px;
  }
  .fieldset-special-1{
    margin-top: -70px;
  }
  .fieldset-special-2{
    margin-top: -30px;
  }
  .fieldset-special-3{
    margin-top: -70px!important;
  }
  .custom-drop{
    float:right;
  }
  .hour-field{
    padding-top:0px;
  }
</style>

