<template>
  <v-container>
    <v-row>
      <v-toolbar
          flat
        >
      <v-btn
        icon
        @click="$refs.calendar.prev()"
      >
        <v-icon>mdi-chevron-left</v-icon>
      </v-btn>
      <v-btn
        icon
        @click="$refs.calendar.next()"
      >
        <v-icon>mdi-chevron-right</v-icon>
      </v-btn>
      <v-toolbar-title v-if="$refs.calendar">
        {{ selectedDate }}
      </v-toolbar-title>
      <v-spacer></v-spacer>
      <v-select
        v-model="type"
        :items="types"
        dense
        outlined
        hide-details
        class="ma-2"
        label="calendar type"
      ></v-select>
      <v-btn
        outlined
        class="mr-4"
        color="green"
        @click="jumpToWuulf"
      >
        Jump To WUULF Week
      </v-btn>

      <v-btn
        outlined
        class=""
        color="green"
        @click="setToday"
      >
        Today
      </v-btn>
      </v-toolbar>
    </v-row>
    <v-row justify="center" align="center">
      <v-col cols="12">
        <v-sheet height="850">
        <v-calendar
          ref="calendar"
          v-model="value"
          :weekdays="weekday"
          :type="type"
          :events="events"
          event-overlap-mode="stack"
          :event-color="getEventColor"
        ></v-calendar>
        </v-sheet>
      </v-col>
    </v-row>
    <v-row justify="center">
      <v-col cols="12" md="8" lg="6">
        <v-card>
          <v-card-title>Legend</v-card-title>
          <v-card-text>
            <v-chip v-for="item in eventColors" :color="item.color" :key="item.eventType">
                {{ item.eventType }}
            </v-chip>
          </v-card-text>
        </v-card>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
  export default {
    name: 'WuulfCalendar',
    data() {
      return {
        value: '2022-06-20 13:00',
        weekday: [1, 2, 3, 4, 5, 6, 0],
        type: 'week',
        types: ['week', 'day'],
        events: [
          {
            name: 'Check In',
            start: '2022-06-20 15:00',
            end: '2022-06-20 17:00',
            type: 'Community'
          },
          {
            name: 'Ghost Ranch Tour for New WUULFies',
            start: '2022-06-20 17:00',
            end: '2022-06-20 17:30',
            type: 'Ghost Ranch Activity'
          },
          {
            name: 'Dinner',
            start: '2022-06-20 17:30',
            end: '2022-06-20 19:00',
            type: 'Meal'
          },
          {
            name: 'Orientation & Late Registration',
            start: '2022-06-20 19:00',
            end: '2022-06-20 21:00',
            type: 'Community'
          },
          {
            name: 'Music & Sing-a-long',
            start: '2022-06-20 21:00',
            end: '2022-06-20 22:00',
            type: 'Activity'
          },
          {
            name: 'Ghost Ranch Quiet Hours',
            start: '2022-06-20 22:00',
            end: '2022-06-20 23:00',
            type: 'Ghost Ranch Activity'
          },

          {
            name: 'Breakfast',
            start: '2022-06-21 7:30',
            end: '2022-06-21 8:00',
            type: 'Meal'
          },
          {
            name: 'Community Gathering',
            start: '2022-06-21 8:15',
            end: '2022-06-21 9:00',
            type: 'Community'
          },
          {
            name: 'Program Day',
            start: '2022-06-21 9:00',
            end: '2022-06-21 16:30',
            type: 'Youth Programming'
          },
          
          {
            name: 'Lunch',
            start: '2022-06-21 12:00',
            end: '2022-06-21 13:00',
            type: 'Meal'
          },
          {
            name: 'Swim',
            start: '2022-06-21 13:00',
            end: '2022-06-21 16:30',
            type: 'Activity'
          },
          {
            name: 'Craft: Water Colors w/ Rick Wells',
            start: '2022-06-21 13:00',
            end: '2022-06-21 14:30',
            type: 'Activity'
          },
          {
            name: 'Talk: Braiding Sweetgrass w/ Jane Everham',
            start: '2022-06-21 14:30',
            end: '2022-06-21 16:30',
            type: 'Activity'
          },
          {
            name: 'Happy Hour & Community Time',
            start: '2022-06-21 16:30',
            end: '2022-06-21 17:30',
            type: 'Community'
          },
          {
            name: 'Dinner',
            start: '2022-06-21 17:30',
            end: '2022-06-21 19:00',
            type: 'Meal'
          },
          {
            name: 'Game Night',
            start: '2022-06-21 19:00',
            end: '2022-06-21 21:00',
            type: 'Community'
          },
          {
            name: 'Music & Sing-a-long',
            start: '2022-06-21 21:00',
            end: '2022-06-21 22:00',
            type: 'Activity'
          },
          {
            name: 'Ghost Ranch Quiet Hours',
            start: '2022-06-21 22:00',
            end: '2022-06-21 23:00',
            type: 'Ghost Ranch Activity'
          },

          {
            name: 'Breakfast',
            start: '2022-06-22 7:30',
            end: '2022-06-22 8:00',
            type: 'Meal'
          },
          {
            name: 'Community Gathering',
            start: '2022-06-22 8:15',
            end: '2022-06-22 9:00',
            type: 'Community'
          },
          {
            name: 'Offsite',
            start: '2022-06-22 9:00',
            end: '2022-06-22 12:00',
            type: 'Activity'
          },
          {
            name: 'Shamshoian, Armenian Dance',
            start: '2022-06-22 9:00',
            end: '2022-06-22 10:30',
            type: 'Activity'
          },
          {
            name: 'Talk: Book Club',
            start: '2022-06-22 10:30',
            end: '2022-06-22 12:00',
            type: 'Activity'
          },
          {
            name: 'Hike: TBD',
            start: '2022-06-22 9:00',
            end: '2022-06-22 12:00',
            type: 'Activity'
          },
          {
            name: 'Lunch',
            start: '2022-06-22 12:00',
            end: '2022-06-22 13:00',
            type: 'Meal'
          },
          {
            name: 'Swim',
            start: '2022-06-22 13:00',
            end: '2022-06-22 16:30',
            type: 'Activity'
          },
          {
            name: 'Craft: Jewelry w/ Peggy',
            start: '2022-06-22 13:00',
            end: '2022-06-22 14:30',
            type: 'Activity'
          },
          {
            name: 'Talk: Reser, Climate',
            start: '2022-06-22 14:30',
            end: '2022-06-22 16:30',
            type: 'Activity'
          },
          {
            name: 'Henna under tree',
            start: '2022-06-22 13:00',
            end: '2022-06-22 14:30',
            type: 'Activity'
          },
          {
            name: 'Happy Hour & Community Time',
            start: '2022-06-22 16:30',
            end: '2022-06-22 17:30',
            type: 'Community'
          },
          {
            name: 'Dinner',
            start: '2022-06-22 17:30',
            end: '2022-06-22 19:00',
            type: 'Meal'
          },
          {
            name: 'Solstice Service',
            start: '2022-06-22 19:00',
            end: '2022-06-22 21:00',
            type: 'Community'
          },
          {
            name: 'Music & Sing-a-long',
            start: '2022-06-22 21:00',
            end: '2022-06-22 22:00',
            type: 'Activity'
          },
          {
            name: 'Ghost Ranch Quiet Hours',
            start: '2022-06-22 22:00',
            end: '2022-06-22 23:00',
            type: 'Ghost Ranch Activity'
          },

          {
            name: 'Breakfast',
            start: '2022-06-23 7:30',
            end: '2022-06-23 8:00',
            type: 'Meal'
          },
          {
            name: 'Community Gathering',
            start: '2022-06-23 8:15',
            end: '2022-06-23 9:00',
            type: 'Community'
          },
          {
            name: 'Program Day',
            start: '2022-06-23 9:00',
            end: '2022-06-23 16:30',
            type: 'Youth Programming'
          },
          {
            name: 'Lunch',
            start: '2022-06-23 12:00',
            end: '2022-06-23 13:00',
            type: 'Meal'
          },
          {
            name: 'Swim',
            start: '2022-06-23 13:00',
            end: '2022-06-23 16:30',
            type: 'Activity'
          },
          {
            name: 'Craft: Tie Dye',
            start: '2022-06-23 13:00',
            end: '2022-06-23 14:30',
            type: 'Activity'
          },
          {
            name: 'Talk: Eldredge, Ageism',
            start: '2022-06-23 14:30',
            end: '2022-06-23 16:30',
            type: 'Activity'
          },
          {
            name: 'Happy Hour & Community Time',
            start: '2022-06-23 16:30',
            end: '2022-06-23 17:30',
            type: 'Community'
          },
          {
            name: 'Dinner',
            start: '2022-06-23 17:30',
            end: '2022-06-23 19:00',
            type: 'Meal'
          },
          {
            name: 'Karoke Night',
            start: '2022-06-23 19:00',
            end: '2022-06-23 21:00',
            type: 'Community'
          },
          {
            name: 'Music & Sing-a-long',
            start: '2022-06-23 21:00',
            end: '2022-06-23 22:00',
            type: 'Activity'
          },
          {
            name: 'Ghost Ranch Quiet Hours',
            start: '2022-06-23 22:00',
            end: '2022-06-23 23:00',
            type: 'Ghost Ranch Activity'
          },

          {
            name: 'Breakfast',
            start: '2022-06-24 7:30',
            end: '2022-06-24 8:00',
            type: 'Meal'
          },
          {
            name: 'Community Gathering',
            start: '2022-06-24 8:15',
            end: '2022-06-24 9:00',
            type: 'Community'
          },
          {
            name: 'Offsite',
            start: '2022-06-24 9:00',
            end: '2022-06-24 12:00',
            type: 'Activity'
          },
          {
            name: 'Talk: TBD',
            start: '2022-06-24 10:30',
            end: '2022-06-24 12:00',
            type: 'Activity'
          },
          {
            name: 'Labrynth Walk w/ Jane Everham',
            start: '2022-06-24 9:00',
            end: '2022-06-24 10:30',
            type: 'Activity'
          },
          {
            name: 'Photography w/ Mark',
            start: '2022-06-24 9:00',
            end: '2022-06-24 10:30',
            type: 'Activity'
          },
          {
            name: 'Hike TBD',
            start: '2022-06-24 9:00',
            end: '2022-06-24 12:00',
            type: 'Activity'
          },
          {
            name: 'Lunch',
            start: '2022-06-24 12:00',
            end: '2022-06-24 13:00',
            type: 'Meal'
          },
          {
            name: 'Craft: Painting w/ Cindy',
            start: '2022-06-24 13:00',
            end: '2022-06-24 14:30',
            type: 'Activity'
          },
          {
            name: 'Talk: John Rowe, Positivity',
            start: '2022-06-24 14:30',
            end: '2022-06-24 16:30',
            type: 'Activity'
          },
          {
            name: 'Lake Swim (parent supervision required)',
            start: '2022-06-24 13:00',
            end: '2022-06-24 16:30',
            type: 'Activity'
          },
          {
            name: 'Happy Hour & Community Time',
            start: '2022-06-24 16:30',
            end: '2022-06-24 17:30',
            type: 'Community'
          },
          {
            name: 'Dinner',
            start: '2022-06-24 17:30',
            end: '2022-06-24 19:00',
            type: 'Meal'
          },
          {
            name: 'Dance Night',
            start: '2022-06-24 19:00',
            end: '2022-06-24 21:00',
            type: 'Community'
          },
          {
            name: 'Music & Sing-a-long',
            start: '2022-06-24 21:00',
            end: '2022-06-24 22:00',
            type: 'Activity'
          },
          {
            name: 'Ghost Ranch Quiet Hours',
            start: '2022-06-24 22:00',
            end: '2022-06-24 23:00',
            type: 'Ghost Ranch Activity'
          },

          {
            name: 'Breakfast',
            start: '2022-06-25 7:30',
            end: '2022-06-25 8:00',
            type: 'Meal'
          },
          {
            name: 'Community Gathering',
            start: '2022-06-25 8:15',
            end: '2022-06-25 9:00',
            type: 'Community'
          },
          {
            name: 'Program Day',
            start: '2022-06-25 9:00',
            end: '2022-06-25 16:30',
            type: 'Youth Programming'
          },
          {
            name: 'Lunch',
            start: '2022-06-25 12:00',
            end: '2022-06-25 13:00',
            type: 'Meal'
          },
          {
            name: 'Happy Hour & Community Time',
            start: '2022-06-25 16:30',
            end: '2022-06-25 17:30',
            type: 'Community'
          },
          {
            name: 'Dinner',
            start: '2022-06-25 17:30',
            end: '2022-06-25 19:00',
            type: 'Meal'
          },
          {
            name: 'Talent/No Talent Show',
            start: '2022-06-25 19:00',
            end: '2022-06-25 21:00',
            type: 'Community'
          },
          {
            name: 'Music & Sing-a-long',
            start: '2022-06-25 21:00',
            end: '2022-06-25 22:00',
            type: 'Activity'
          },
          {
            name: 'Ghost Ranch Quiet Hours',
            start: '2022-06-25 22:00',
            end: '2022-06-25 23:00',
            type: 'Ghost Ranch Activity'
          },

          {
            name: 'Breakfast',
            start: '2022-06-26 7:30',
            end: '2022-06-26 8:00',
            type: 'Meal'
          },
          {
            name: 'Community Gathering Fairwell',
            start: '2022-06-26 8:45',
            end: '2022-06-26 10:00',
            type: 'Community'
          },
        ],
        eventColors: [
          {
            eventType: 'Meal',
            color: 'purple'
          },
          {
            eventType: 'Community',
            color: 'green'
          },
          {
            eventType: 'Activity',
            color: 'orange'
          },
          {
            eventType: 'Ghost Ranch Activity',
            color: 'blue'
          },
          {
            eventType: 'Youth Programming',
            color: 'teal'
          },
        ]

      }
    },
    computed: {
      selectedDate() {
        return (this.$refs.calendar) ? this.$refs.calendar.title : '' 
      }
    },
    methods: {
      setToday() {
        this.value = ''
      },
      jumpToWuulf() {
        this.value = '2022-06-20 13:00'
      },
      getEventColor(event) {
        // default color is black
        var color = 'black'
        this.eventColors.forEach(element => {
          if (event.type == element.eventType){
            color = element.color
          }
        })
        return color
      }

    },
  }
</script>

<style lang="scss" scoped>

</style>