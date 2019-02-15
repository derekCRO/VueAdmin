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
          <v-subheader>  Component for CRUD</v-subheader>
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
              <v-btn slot="activator" color="primary" dark class="mb-2">New Code</v-btn>
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
                        <!-- <v-text-field v-model="editedItem.category" label="Category"></v-text-field> -->
                        <v-select
                          :items="categories"
                          v-model="editedItem.category"
                          label="Category"
                          item-text="category"
                          item-value="description"
                          single-line
                        ></v-select>
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
          <v-subheader> </v-subheader>  
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
                <v-card-text >
                  <v-layout row wrap class="tab1">
                    <v-flex xs6 sm6 lg3>
                      <v-subheader >Category</v-subheader>
                    </v-flex>
                    <v-flex xs6 sm6 lg3 d-flex>
                      <v-select
                        :items="categories"
                        box
                        label="Category"
                      ></v-select>
                    </v-flex>
                  </v-layout>
                  <v-layout row wrap class="tab1">
                    <v-flex xs6 sm6 lg3>
                      <v-subheader>Default Role Type</v-subheader>
                    </v-flex>
                    <v-flex xs6 sm6 lg3 d-flex>
                      <v-select
                        :items="items"
                        label=""
                      ></v-select>
                    </v-flex>
                    <v-flex xs6 sm6 lg3>
                      <v-text-field
                        label=""
                      ></v-text-field>
                    </v-flex>
                  </v-layout>
                  <v-layout row wrap class="tab1">
                    <v-flex xs6 sm6 lg3>
                      <v-subheader>Reest Break Length</v-subheader>
                    </v-flex>
                    <v-flex xs6 sm6 lg3>
                      <v-text-field
                        label=""
                      ></v-text-field>
                    </v-flex>
                  </v-layout>
                  <v-layout row wrap>
                    <v-flex xs6 sm6 lg3>
                      <v-subheader>Shift Length</v-subheader>
                    </v-flex>
                    <v-flex xs6 sm6 lg3>
                      <v-text-field
                        label=""
                      ></v-text-field>
                    </v-flex>
                  </v-layout>
                  <v-layout row wrap class="tab1">
                    <v-flex xs6 sm6 lg3>
                      <v-subheader></v-subheader>
                    </v-flex>
                    <v-flex xs6 sm6 lg3>
                      <v-checkbox
                        v-model="checkbox"
                        label="Exclude from Aggregate Hours Calculation"
                      ></v-checkbox>
                    </v-flex>
                  </v-layout>
                  <v-layout row wrap class="tab1">
                    <v-flex xs6 sm6 lg3>
                      <v-subheader></v-subheader>
                    </v-flex>
                    <v-flex xs6 sm6 lg3>
                      <v-checkbox
                        v-model="checkbox"
                        label="Include On-Call Log"
                      ></v-checkbox>
                    </v-flex>
                  </v-layout>
                  <v-layout row wrap class="tab1">
                    <v-flex xs6 sm6 lg3>
                      <v-subheader></v-subheader>
                    </v-flex>
                    <v-flex xs6 sm6 lg3>
                      <v-checkbox
                        v-model="checkbox"
                        label="Hold Standard Shift Code"
                      ></v-checkbox>
                    </v-flex>
                  </v-layout>
                  <v-layout row wrap class="tab1">
                    <v-flex xs6 sm6 lg3>
                      <v-subheader></v-subheader>
                    </v-flex>
                    <v-flex xs6 sm6 lg3 >
                      <v-checkbox
                        v-model="checkbox"
                        label="Requires an Accompanying Shift"
                      ></v-checkbox>
                    </v-flex>
                  </v-layout>
                  <v-layout row wrap class="tab1">
                    <v-flex xs6 sm6 lg3>
                      <v-subheader></v-subheader>
                    </v-flex>
                    <v-flex xs6 sm6 lg4 pt-4 pl-5>
                      <v-layout>
                        <v-subheader>Companion shift must be a minimum of </v-subheader>
                        <v-text-field
                          label="hours"
                          class="hour-field"
                        ></v-text-field>
                      </v-layout>
                    </v-flex>
                  </v-layout>
                  <v-layout row wrap class="tab1">
                    <v-flex xs6 sm6 lg3>
                      <v-subheader></v-subheader>
                    </v-flex>
                    <v-flex xs6 sm6 lg3 class="tab1-special">
                      <v-checkbox
                        v-model="checkbox"
                        label="Update Employees' Briefed Status"
                      ></v-checkbox>
                    </v-flex>
                  </v-layout>
                  <v-layout class="justify-center mr-2">
                    <v-btn
                      color="info"
                      class="white--text"
                    >
                      Save
                      <v-icon right dark>save</v-icon>
                    </v-btn>
                    <v-btn
                      color="red"
                      class="white--text"
                    >
                      Close
                      <v-icon right dark>close</v-icon>
                    </v-btn>
                  </v-layout>
                </v-card-text>
              </v-card>
            </v-tab-item>
            <v-tab-item
              :key="2"
              :id="'tab-' + 2"
            >
              <v-card flat>
                <v-card-text>
                  <v-layout row wrap>
                    <v-flex xs6 sm6 lg3 >
                      <v-subheader class="custom-drop">Use Earning Code</v-subheader>
                    </v-flex>
                    <v-flex xs6 sm6 lg3 d-flex>
                      <v-select
                        :items="items"
                        label=""
                      ></v-select>
                    </v-flex>
                    <v-flex xs6 sm6 lg3>
                      <v-text-field
                        label=""
                      ></v-text-field>
                    </v-flex>
                  </v-layout>
                  <v-layout  row wrap> 
                    <v-flex xs6 sm6 lg7>
                      <fieldset style="padding:30px">
                        <legend>
                          <v-flex xs6 sm6 lg12 >
                            <v-checkbox
                              v-model="checkbox"
                              label=" Pay Normal Hours"
                            ></v-checkbox>
                          </v-flex>
                        </legend>
                        <v-flex xs6 sm6 lg12 class="tab2 fieldset-special-1">
                          <v-checkbox
                            v-model="checkbox"
                            label="Apply Award (calculate allowances, penalties, loadings etc)"
                          ></v-checkbox>
                        </v-flex>
                        <v-flex xs6 sm6 lg12 class="tab2">
                          <v-checkbox
                            v-model="checkbox"
                            label="Pay Meal Allowane"
                          ></v-checkbox>
                        </v-flex>
                        <v-flex xs6 sm6 lg12 class="tab2">
                          <v-checkbox
                            v-model="checkbox"
                            label="Flat Amount (pay the normal hourly rate as a one off amount"
                          ></v-checkbox>
                        </v-flex>
                        <v-flex xs6 sm6 lg12 class="tab2">
                          <v-checkbox
                            v-model="checkbox"
                            label="Exclude Allowances (ignore allowances on the pay class)"
                          ></v-checkbox>
                        </v-flex>
                      </fieldset>
                    </v-flex>
                    <v-flex xs6 sm6 lg5>
                      <fieldset style="padding:30px">
                        <legend>
                          <v-flex xs6 sm6 lg12>
                            <v-checkbox
                              v-model="checkbox"
                              label=" Other Pay Options"
                            ></v-checkbox>
                          </v-flex>
                        </legend>
                        
                        <v-flex xs6 sm6 lg12 class="tab2 fieldset-special-1">
                          <v-checkbox
                            v-model="checkbox"
                            label="Pay By Task"
                          ></v-checkbox>
                        </v-flex>
                        <v-flex xs6 sm6 lg12 class="tab2">
                          <v-checkbox
                            v-model="checkbox"
                            label="Pay Standard Position"
                          ></v-checkbox>
                        </v-flex>
                        <v-flex xs6 sm6 lg12 class="tab2">
                          <v-checkbox
                            v-model="checkbox"
                            label="Voluntary Overtime"
                          ></v-checkbox>
                        </v-flex>
                        <v-flex xs6 sm6 lg12 class="tab2">
                          <v-checkbox
                            v-model="checkbox"
                            label="Zero Pay"
                          ></v-checkbox>
                        </v-flex>
                      </fieldset>
                    </v-flex>
                  </v-layout>
                  <v-layout  row wrap> 
                    <v-flex xs6 sm6 lg7>
                      <fieldset style="padding:30px">
                        <legend>
                          Pay as Leave (select one type of leave only)
                        </legend>
                        <v-layout row wrap>
                          <v-flex xs6 sm6 lg4 class="tab2 fieldset-special-2">
                            <v-checkbox
                              v-model="checkbox"
                              label="Annual Leave"
                            ></v-checkbox>
                          </v-flex>
                          <v-flex xs6 sm6 lg4 class="tab2 fieldset-special-2">
                            <v-checkbox
                              v-model="checkbox"
                              label="Workers Compensation"
                            ></v-checkbox>
                          </v-flex>
                        </v-layout>
                        <v-layout row wrap ml-2 >
                          <v-flex xs6 sm6 lg4 class="tab2">
                            <v-checkbox
                              v-model="checkbox"
                              label="Pay Leave Loading"
                            ></v-checkbox>
                          </v-flex>
                          <v-flex xs6 sm6 lg4 class="tab2">
                            <v-checkbox
                              v-model="checkbox"
                              label="Accrue Time In Lieu or"
                            ></v-checkbox>
                          </v-flex>
                          <v-flex xs6 sm6 lg4 class="tab2">
                            <v-checkbox
                              v-model="checkbox"
                              label="Pay Time In Liue"
                            ></v-checkbox>
                          </v-flex>
                        </v-layout>
                        <v-layout row wrap>
                          <v-flex xs6 sm6 lg4 class="tab2">
                            <v-checkbox
                              v-model="checkbox"
                              label="Sick Leave"
                            ></v-checkbox>
                          </v-flex>
                        </v-layout>
                        <v-layout row wrap>
                          <v-flex xs6 sm6 lg4 class="tab2">
                            <v-checkbox
                              v-model="checkbox"
                              label="Long Service"
                            ></v-checkbox>
                          </v-flex>
                        </v-layout>
                        <v-layout row wrap>
                          <v-flex xs6 sm6 lg4 class="tab2">
                            <v-checkbox
                              v-model="checkbox"
                              label="Rostered Day Off"
                            ></v-checkbox>
                          </v-flex>
                        </v-layout>
                      </fieldset>
                    </v-flex>
                    <v-flex xs6 sm6 lg5 >
                      <fieldset style="padding:30px">
                        <legend>
                          Other Parameters
                        </legend>
                        
                        <v-flex xs6 sm6 lg12 class="tab2 fieldset-special-2">
                          <v-checkbox
                            v-model="checkbox"
                            label="Charge the Client for this shift"
                          ></v-checkbox>
                        </v-flex>
                        <v-flex xs6 sm6 lg12 class="tab2">
                          <v-checkbox
                            v-model="checkbox"
                            label="Exclude From Timeclock"
                          ></v-checkbox>
                        </v-flex>
                        <v-flex xs6 sm6 lg12 class="tab2">
                          <v-checkbox
                            v-model="checkbox"
                            label="Exclude When Recosting"
                          ></v-checkbox>
                        </v-flex>
                        <v-flex xs6 sm6 lg12 class="tab2">
                          <v-checkbox
                            v-model="checkbox"
                            label="Include for Overtime"
                          ></v-checkbox>
                        </v-flex>
                        <v-flex xs6 sm6 lg12 class="tab2">
                          <v-checkbox
                            v-model="checkbox"
                            label="Include in profit and Loss"
                          ></v-checkbox>
                        </v-flex>
                      </fieldset>
                    </v-flex>
                  </v-layout>
                  <v-layout class="justify-center mr-2">
                    <v-btn
                      color="info"
                      class="white--text"
                    >
                      Save
                      <v-icon right dark>save</v-icon>
                    </v-btn>
                    <v-btn
                      color="red"
                      class="white--text"
                    >
                      Close
                      <v-icon right dark>close</v-icon>
                    </v-btn>
                  </v-layout>
                </v-card-text>
              </v-card>
            </v-tab-item>
            <v-tab-item
              :key="3"
              :id="'tab-' + 3"
            >
              <v-card flat>
                <v-card-text>
                  <v-layout row wrap>
                    <v-flex xs6 sm6 lg3>
                      <v-subheader class="custom-drop">Invoicing Description</v-subheader>
                    </v-flex>
                    <v-flex xs6 sm6 lg3>
                      <v-text-field
                        label=""
                      ></v-text-field>
                    </v-flex>
                  </v-layout>
                  <v-layout row wrap class="tab3">
                    <v-flex xs6 sm6 lg3>
                      <v-subheader></v-subheader>
                    </v-flex>
                    <v-flex xs6 sm6 lg3>
                      <v-checkbox
                        v-model="checkbox"
                        label="Do not show on Dashboard"
                      ></v-checkbox>
                    </v-flex>
                  </v-layout>
                  <v-layout row wrap class="tab3">
                    <v-flex xs6 sm6 lg3>
                      <v-subheader></v-subheader>
                    </v-flex>
                    <v-flex xs6 sm6 lg3>
                      <v-checkbox
                        v-model="checkbox"
                        label="Inactive on SWB dropdown"
                      ></v-checkbox>
                    </v-flex>
                  </v-layout>
                  <v-layout row wrap class="tab3">
                    <v-flex xs6 sm6 lg3>
                      <v-subheader></v-subheader>
                    </v-flex>
                    <v-flex xs6 sm6 lg3 >
                      <v-checkbox
                        v-model="checkbox"
                        label="Internal Use Only"
                      ></v-checkbox>
                    </v-flex>
                  </v-layout>
                  <v-layout row wrap class="tab3">
                    <v-flex xs6 sm6 lg3>
                      <v-subheader></v-subheader>
                    </v-flex>
                    <v-flex xs6 sm6 lg3>
                      <v-checkbox
                        v-model="checkbox"
                        label="Omit from Confirmed Shift reporting"
                      ></v-checkbox>
                    </v-flex>
                  </v-layout>
                  <v-layout row wrap class="tab3">
                    <v-flex xs6 sm6 lg3>
                      <v-subheader></v-subheader>
                    </v-flex>
                    <v-flex xs6 sm6 lg3>
                      <v-checkbox
                        v-model="checkbox"
                        label="Omit from Hours Worked reporting"
                      ></v-checkbox>
                    </v-flex>
                  </v-layout>
                  <v-layout row wrap class="tab3">
                    <v-flex xs6 sm6 lg3>
                      <v-subheader></v-subheader>
                    </v-flex>
                    <v-flex xs6 sm6 lg3>
                      <v-checkbox
                        v-model="checkbox"
                        label="Show Rostering Description"
                      ></v-checkbox>
                    </v-flex>
                  </v-layout>
                  <v-layout row wrap class="tab3">
                    <v-flex xs6 sm6 lg3>
                      <v-subheader></v-subheader>
                    </v-flex>
                    <v-flex xs6 sm6 lg3>
                      <v-checkbox
                        v-model="checkbox"
                        label="Report as a No Fill"
                      ></v-checkbox>
                    </v-flex>
                  </v-layout>
                  <v-layout class="justify-center" >
                    <v-flex xs6 sm6 lg6 ml-2>
                      <fieldset style="padding:30px">
                        <legend>
                          <v-flex xs6 sm6 lg12>
                            <v-checkbox
                              v-model="checkbox"
                              label=" Automate Significant Event (Ctrl-E)"
                            ></v-checkbox>
                          </v-flex>
                        </legend>
                        
                        <v-layout row wrap class="tab3 fieldset-special-3">
                          <v-flex xs6 sm6 lg6>
                            <v-subheader>Ctrl-E Category</v-subheader>
                          </v-flex>
                          <v-flex xs6 sm6 lg6 d-flex>
                            <v-select
                              :items="items"
                              label=""
                            ></v-select>
                          </v-flex>
                        </v-layout>
                        <v-layout row wrap class="tab3">
                          <v-flex xs6 sm6 lg6>
                            <v-subheader>Ctrl-E Template</v-subheader>
                          </v-flex>
                          <v-flex xs6 sm6 lg6 d-flex>
                            <v-select
                              :items="items"
                              label=""
                            ></v-select>
                          </v-flex>
                        </v-layout>
                      </fieldset>
                    </v-flex>
                  </v-layout>
                  <v-layout class="justify-center mr-2">
                    <v-btn
                      color="info"
                      class="white--text"
                    >
                      Save
                      <v-icon right dark>save</v-icon>
                    </v-btn>
                    <v-btn
                      color="red"
                      class="white--text"
                    >
                      Close
                      <v-icon right dark>close</v-icon>
                    </v-btn>
                  </v-layout>
                </v-card-text>
              </v-card>
            </v-tab-item>
          </v-tabs>
                 
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
    items: ['Training', 'Scheduled Shift', 'Annual Leave', 'Other'],
    categories: ['Annual Leave','Leave Without pay','Long Service Leave','Meeting','No Show','Other','Other(nRDO)','RDO','Scheduled Shift','Sick','Sleepover','Training','Workers Comp'],
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

