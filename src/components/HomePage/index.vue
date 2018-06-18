<template>
  <div>
    <div class="header">
      <div class="logo">
        HaveFun
      </div>
      <div class="explore">
        <font-awesome-icon class="search-icon" icon="search" />
        <!-- <input v-model="exploreFilter" type="text" placeholder="Explore your own activities"> -->
      </div>
    </div>
    <div class="content">
      <div class="sidebar">
        <div class="sidebar-container">
          <div class="sidebar-location" :class="{'sidebar-unfold': sidebarIcon[0].sidebarPlusIcon}">
            <div class="sidebar-title">
              Location
              <div class="sidebar-title-mobile-icon" @click="mobileIconControl(0)">
                <font-awesome-icon v-if="sidebarIcon[0].sidebarPlusIcon" class="plus-icon" icon="plus" />
                <font-awesome-icon v-if="sidebarIcon[0].sidebarMinusIcon" class="minus-icon" icon="minus" />
              </div>
            </div>
            <select class="" name="" v-model="zoneFilter">
              <option value="all">All</option>
              <option v-for="zone in activitiesZone" :value="zone">
                {{zone}}
              </option>
            </select>
          </div>
          <div class="sidebar-divider">
          </div>
          <div class="sidebar-date" :class="{'sidebar-unfold': sidebarIcon[1].sidebarPlusIcon}">
            <div class="sidebar-title">
              Date //尚未實作
              <div class="sidebar-title-mobile-icon" @click="mobileIconControl(1)">
                <font-awesome-icon v-if="sidebarIcon[1].sidebarPlusIcon" class="plus-icon" icon="plus" />
                <font-awesome-icon v-if="sidebarIcon[1].sidebarMinusIcon" class="minus-icon" icon="minus" />
              </div>
            </div>
            <div class="sidebar-date-form">
              <div class="form-title">
                <label for="from">from</label>
                <label for="to">to</label>
              </div>
              <div class="form-input">
                <input type="text" name="from" value="">
                <input type="text" name="to" value="">
              </div>
            </div>
          </div>
          <div class="sidebar-divider">
          </div>
          <div class="sidebar-categories" :class="{'sidebar-unfold': sidebarIcon[2].sidebarPlusIcon}">
            <div class="sidebar-title">
              Categories
              <div class="sidebar-title-mobile-icon" @click="mobileIconControl(2)">
                <font-awesome-icon v-if="sidebarIcon[2].sidebarPlusIcon" class="plus-icon" icon="plus" />
                <font-awesome-icon v-if="sidebarIcon[2].sidebarMinusIcon" class="minus-icon" icon="minus" />
              </div>
            </div>
            <div class="sidebar-categories-form">
              <div>
                <input id="is-free" type="checkbox" v-model="isFreeFilter">
                <label for="is-free">免費參觀</label>
              </div>
              <div>
                <input id="not-free" type="checkbox" v-model="notFreeFilter">
                <label for="not-free">非免費參觀</label>
              </div>
            </div>
          </div>
        </div>
      </div>


      <div class="main">
        <div class="main-title-text">
          Showing <span>{{activitiesResult.length}}</span> results by...
        </div>
        <div class="main-title-tags">
          <div class="title-tag" v-if="isFreeFilter">
            免費參觀
            <font-awesome-icon @click="setIsFreeFilter" class="times-icon" icon="times" />
          </div>
          <div class="title-tag" @click="setNotFreeFilter" v-if="notFreeFilter">
            非免費參觀
            <font-awesome-icon class="times-icon" icon="times" />
          </div>
        </div>
        <div class="activities">
          <div class="activity" v-for="activity in activitiesResult">
            <div class="activity-image" :style="{backgroundImage: 'url(' + activity.Picture1 + ')'}">

            </div>
            <div class="main-content">
              <div class="activity-title">
                {{activity.Name}}
              </div>
              <div class="activity-describe">
                {{activity.Description}}
                <div class="describe-readmore">
                  ...
                </div>
              </div>
              <div class="activity-detail">
                <div class="activity-organizer">
                  {{activity.Name}}
                </div>
                <div class="activity-tag">
                  {{activity.Ticketinfo ? activity.Ticketinfo : '非免費參觀'}}
                </div>
              </div>
              <div class="activity-detail">
                <div class="activity-address">
                  <font-awesome-icon class="search-icon" icon="search" />
                  {{activity.Zone}}
                </div>
                <div class="activity-date">
                  <font-awesome-icon class="search-icon" icon="calendar-alt" />
                  {{activity.Changetime}}
                </div>
              </div>
            </div>
          </div>
        </div>

      </div>
    </div>
  </div>
</template>

<script>
import FontAwesomeIcon from '@fortawesome/vue-fontawesome'
import axios from 'axios'
export default {
  name: 'HelloWorld',
  components: {
    FontAwesomeIcon
  },
  data () {
    return {
      activities: [],
      activitiesZone: [],
      categories: '',
      isFreeFilter: false,
      notFreeFilter: false,
      exploreFilter: '',
      zoneFilter: 'all',
      sidebarIcon: [
        {
          sidebarPlusIcon: false,
          sidebarMinusIcon: true
        },
        {
          sidebarPlusIcon: false,
          sidebarMinusIcon: true
        },
        {
          sidebarPlusIcon: false,
          sidebarMinusIcon: true
        }
      ]

    }
  },
  mounted () {
    axios.get('https://data.kcg.gov.tw/api/action/datastore_search?resource_id=92290ee5-6e61-456f-80c0-249eae2fcc97&limit=200')
      .then((response) => {
        this.activities = response.data.result.records
        response.data.result.records.forEach((activity) => {
          if (!this.activitiesZone.includes(activity.Zone)) {
            this.activitiesZone.push(activity.Zone)
          }
        })
      })
  },
  computed: {
    activitiesResult () {
      let activitiesResult = this.activities
      if (this.isFreeFilter) {
        activitiesResult = activitiesResult.filter((activity) => {
          if (activity.Ticketinfo === '免費參觀') {
            return true
          } else {
            return false
          }
        })
      }
      if (this.notFreeFilter) {
        activitiesResult = activitiesResult.filter((activity) => {
          if (activity.Ticketinfo !== '免費參觀') {
            return true
          } else {
            return false
          }
        })
      }
      if (this.exploreFilter !== '') {
        activitiesResult = activitiesResult.filter((activity) => {
          if (activity.Name.includes(this.exploreFilter)) {
            return true
          } else {
            return false
          }
        })
      }
      if (this.zoneFilter !== 'all') {
        activitiesResult = activitiesResult.filter((activity) => {
          if (activity.Zone === this.zoneFilter) {
            return true
          } else {
            return false
          }
        })
      }
      return activitiesResult
    }
  },
  methods: {
    mobileIconControl(index) {
      this.sidebarIcon[index].sidebarPlusIcon = !this.sidebarIcon[index].sidebarPlusIcon
      this.sidebarIcon[index].sidebarMinusIcon = !this.sidebarIcon[index].sidebarPlusIcon
    },
    setIsFreeFilter () {
      this.isFreeFilter = false
    },
    setNotFreeFilter () {
      this.notFreeFilter = false
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="scss" scoped>
  @import './index.scss';
  @import './mediaQuery.scss';
</style>
