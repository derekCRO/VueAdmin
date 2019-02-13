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
                :items="countries"
                v-model="e1"
                label="Select the Code"
                item-text="country"
                item-value="abbr"
                single-line
              ></v-select>
            </v-flex>
            <v-flex xs3>
              <v-text-field
                name="input-4"
                label=""
                value=""
                color="info"
                single-line
              ></v-text-field>
            </v-flex>
          </v-layout>
          <v-tabs icons-and-text centered dark color="cyan">
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
import Countries  from '@/api/country';
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
    countries: Countries,
    color: Material,
    selectedTab: 'tab-1',
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

};
</script>
