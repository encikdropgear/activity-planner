<template>
  <div id="ActivityApp">
    <nav class="navbar is-white topNav">
      <div class="container">
        <div class="navbar-brand">
          <h1>{{ fullAppName }}</h1>
          <!-- <h1>{{ watchedAppName }}</h1> -->
        </div>
      </div>
    </nav>
    <nav class="navbar is-white">
      <div class="container">
        <div class="navbar-menu">
          <div class="navbar-start">
            <a
              class="navbar-item is-active"
              href="#"
            >Newest</a>
            <a
              class="navbar-item"
              href="#"
            >In Progress</a>
            <a
              class="navbar-item"
              href="#"
            >Finished</a>
          </div>
        </div>
      </div>
    </nav>
    <section class="container">
      <div class="columns">
        <div class="column is-3">
          <!-- Activity form -->
          <ActivityCreate @activityCreated="addActivity" :categories="categories" />
          <!-- activity form end -->
        </div>
        <div class="column is-9">
          <div class="box content">
            <ActivityItem
              v-for="activity in activities"
              :key="activity.id"
              :activity="activity"
            />
            <div class="activity-length">
              curretly {{ activityLength }} Activity
            </div>
            <div class="activity-motivation">
              {{ activityMotivation }}
            </div>
          </div>
        </div>
      </div>
    </section>
  </div>
</template>

<script>
import ActivityItem from '@/components/ActivityItem'
import ActivityCreate from '@/components/ActivityCreate'
import { fetchActivities, fetchUser, fetchCategories } from '@/api'
export default {
  name: 'App',
  components: {ActivityItem, ActivityCreate},
  data() {
    return {
      creator: 'Nazrin Lagis',
      appName: 'Activity Planner',
      watchedAppName: 'Activity Planner by Nazrin Lagis',
      items: {
        1: {
          name: 'Amani'
        },
        2: {
          name: 'amanda'
        }
      },
      user: {},
      activities: {},
      categories: {}
    }
  },
  computed: {
    fullAppName () {
      return this.appName + ' by ' + this.creator
    },
    activityLength () {
      return Object.keys(this.activities).length
    },
    activityMotivation () {
      if (this.activityLength && this.activityLength < 5) {
        return 'Nice to see some goals (:'
      } else if (this.activityLength >= 5) {
        return 'So many activities! Good Job!'
      } else {
        return 'No activities, so sad ):'
      }
      }
  },
  // watch: {
  //   creator (val) {
  //     debugger
  //     console.log(val);
  //     this.watchedAppName = this.appName + ' by ' + val
  //   },
  //   appName (val) {
  //     debugger
  //     console.log(val);
  //     this.watchedAppName = val + ' by ' + this.creator
  //   }
  // },
  beforeCreate () {
    console.log('beforeCreate called');
  },
  created () {
    this.activities = fetchActivities()
    this.user = fetchUser()
    this.categories = fetchCategories()
  },
  methods: {
    addActivity (newActivity) {
      this.activities[newActivity.id] = newActivity
      console.log(newActivity);
    }
  }
}
</script>

<style>
#ActivityApp {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
}

html,
body {
  font-family: 'Open Sans', serif;
  background: #F2F6FA;
}

footer {
  background-color: #F2F6FA !important;
}

.activity-motivation {
  float: right;
}

.activity-length {
  display: inline-block;
}

.topNav {
  border-top: 5px solid #3498DB;
}

.topNav .container {
  border-bottom: 1px solid #E6EAEE;
}

.container .columns {
  margin: 3rem 0;
}

.navbar-menu .navbar-item {
  padding: 0 2rem;
}

aside.menu {
  padding-top: 3rem;
}

aside.menu .menu-list {
  line-height: 1.5;
}

aside.menu .menu-label {
  padding-left: 10px;
  font-weight: 700;
}

.button.is-primary.is-alt {
  background: #00c6ff;
  background: -webkit-linear-gradient(to bottom, #0072ff, #00c6ff);
  background: linear-gradient(to bottom, #0072ff, #00c6ff);
  font-weight: 700;
  font-size: 14px;
  height: 3rem;
  line-height: 2.8;
}

.media-left img {
  border-radius: 50%;
}

.media-content p {
  font-size: 14px;
  line-height: 2.3;
  font-weight: 700;
  color: #8F99A3;
}

article.post {
  margin: 1rem;
  padding-bottom: 1rem;
  border-bottom: 1px solid #E6EAEE;
}

article.post:last-child {
  padding-bottom: 0;
  border-bottom: none;
}

.menu-list li {
  padding: 5px;
}

.navbar-brand>h1 {
  font-size: 31px;
  padding: 20px;
}

.example-wrapper {
  margin-left: 30px;
}
</style>
