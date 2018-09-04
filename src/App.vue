<template>
    <v-app id="keep">
      <v-navigation-drawer
        v-model="drawer"
        fixed
        clipped
        class="grey lighten-4"
        app
      >
        <v-list
          dense
          class="grey lighten-4"
        >
          <template v-for="(item, i) in items">
            <v-layout
              v-if="item.heading"
              :key="i"
              row
              align-center
            >
              <v-flex xs6>
                <v-subheader v-if="item.heading">
                  {{ item.heading }}
                </v-subheader>
              </v-flex>
              <v-flex xs6 class="text-xs-right">
                <v-btn small flat>edit</v-btn>
              </v-flex>
            </v-layout>
            <v-divider
              v-else-if="item.divider"
              :key="i"
              dark
              class="my-3"
            ></v-divider>
            <v-list-tile
              v-else
              :key="i"
              @click=""
            >
              <v-list-tile-action>
                <v-icon>{{ item.icon }}</v-icon>
              </v-list-tile-action>
              <v-list-tile-content>
                <v-list-tile-title class="grey--text">
                  {{ item.text }}
                </v-list-tile-title>
              </v-list-tile-content>
            </v-list-tile>
          </template>
        </v-list>
      </v-navigation-drawer>
      
     
      <v-toolbar color="amber" app absolute clipped-left>
        <v-toolbar-side-icon @click.native="drawer = !drawer"></v-toolbar-side-icon>
        <span class="title ml-3 mr-5">Test&nbsp;<span class="font-weight-light">hunter</span></span>
        <v-text-field
          solo-inverted
          flat
          hide-details
          label="Search"
          prepend-inner-icon="search"
        ></v-text-field>
        <v-spacer></v-spacer>
      </v-toolbar>
      <v-content>
          <div>
              <v-card>
                  <v-card-title>
                    Nutrition
                    <v-spacer></v-spacer>
                    <v-text-field
                      v-model="search"
                      append-icon="search"
                      label="Search"
                      single-line
                      hide-details
                    ></v-text-field>
                  </v-card-title>
                  <v-data-table
                    :headers="headers"
                    :items="desserts"
                    :search="search"
                  >
                    <template slot="items" slot-scope="props">
                      <td>{{ props.item.name }}</td>
                      <td class="text-xs-right">{{ props.item.calories }}</td>
                      <td class="text-xs-right">{{ props.item.fat }}</td>
                      <td class="text-xs-right">{{ props.item.carbs }}</td>
                      <td class="text-xs-right">{{ props.item.protein }}</td>
                      <td class="text-xs-right">{{ props.item.iron }}</td>
                    </template>
                    <v-alert slot="no-results" :value="true" color="error" icon="warning">
                      Your search for "{{ search }}" found no results.
                    </v-alert>
                  </v-data-table>
                </v-card>
          </div>
        <!-- <v-container fluid fill-height class="grey lighten-4">
          <v-layout justify-center align-center>
            <v-flex shrink>
              <v-tooltip right>
                <v-btn
                  slot="activator"
                  :href="source"
                  icon
                  large
                  target="_blank"
                >
                  <v-icon large>code</v-icon>
                </v-btn>
                <span>Source</span>
              </v-tooltip>
              <v-tooltip right>
                <v-btn slot="activator" icon large href="https://codepen.io/johnjleider/pen/jZQNbd" target="_blank">
                  <v-icon large>mdi-codepen</v-icon>
                </v-btn>
                <span>Codepen</span>
              </v-tooltip>
            </v-flex>
          </v-layout>
        </v-container> -->
      </v-content>
      <v-layout row justify-center>
          <v-dialog v-model="dialog" persistent max-width="500px">
            <v-btn slot="activator" color="info" dark>Open Dialog</v-btn>
            <v-card>
              <v-card-title>
                <span class="headline">User Profile</span>
              </v-card-title>
              <v-card-text>
                <v-container grid-list-md>
                  <v-layout wrap>
                    <v-flex xs12 sm6 md4>
                      <v-text-field label="Legal first name" required></v-text-field>
                    </v-flex>
                    <v-flex xs12 sm6 md4>
                      <v-text-field label="Legal middle name" hint="example of helper text only on focus"></v-text-field>
                    </v-flex>
                    <v-flex xs12 sm6 md4>
                      <v-text-field
                        label="Legal last name"
                        hint="example of persistent helper text"
                        persistent-hint
                        required
                      ></v-text-field>
                    </v-flex>
                    <v-flex xs12>
                      <v-text-field label="Email" required></v-text-field>
                    </v-flex>
                    <v-flex xs12>
                      <v-text-field label="Password" type="password" required></v-text-field>
                    </v-flex>
                    <v-flex xs12 sm6>
                      <v-select
                        :items="['0-17', '18-29', '30-54', '54+']"
                        label="Age"
                        required
                      ></v-select>
                    </v-flex>
                    <v-flex xs12 sm6>
                      <v-autocomplete
                        :items="['Skiing', 'Ice hockey', 'Soccer', 'Basketball', 'Hockey', 'Reading', 'Writing', 'Coding', 'Basejump']"
                        label="Interests"
                        multiple
                        chips
                      ></v-autocomplete>
                    </v-flex>
                  </v-layout>
                </v-container>
                <small>*indicates required field</small>
              </v-card-text>
              <v-card-actions>
                <v-spacer></v-spacer>
                <v-btn color="blue darken-1" flat @click.native="dialog = false">Close</v-btn>
                <v-btn color="blue darken-1" flat @click.native="dialog = false">Save</v-btn>
              </v-card-actions>
            </v-card>
          </v-dialog>
        </v-layout>
    </v-app>
  </template>
  
  <script>
    export default {
      data: () => ({
        drawer: null,
        items: [
          { icon: 'lightbulb_outline', text: 'Notes' },
          { icon: 'touch_app', text: 'Reminders' },
          { divider: true },
          { heading: 'Labels' },
          { icon: 'add', text: 'Create new label' },
          { divider: true },
          { icon: 'archive', text: 'Archive' },
          { icon: 'delete', text: 'Trash' },
          { divider: true },
          { icon: 'settings', text: 'Settings' },
          { icon: 'chat_bubble', text: 'Trash' },
          { icon: 'help', text: 'Help' },
          { icon: 'phonelink', text: 'App downloads' },
          { icon: 'keyboard', text: 'Keyboard shortcuts' }
        ],dialog: false,
          search: '',
          clipped: false,
          drawer: true,
          fixed: false,
          // items: [
          //   { icon: 'bubble_chart', title: 'Inspire' }
          // ],
          miniVariant: false,
          right: true,
          rightDrawer: false,
          title: 'Vuetify.js',
          headers: [
          {
            text: 'Dessert (100g serving)',
            align: 'left',
            sortable: false,
            value: 'name'
          },
          { text: 'Calories', value: 'calories' },
          { text: 'Fat (g)', value: 'fat' },
          { text: 'Carbs (g)', value: 'carbs' },
          { text: 'Protein (g)', value: 'protein' },
          { text: 'Iron (%)', value: 'iron' }
        ],
        desserts: [
          {
            value: false,
            name: 'Frozen Yogurt',
            calories: 159,
            fat: 6.0,
            carbs: 24,
            protein: 4.0,
            iron: '1%'
          },
          {
            value: false,
            name: 'Ice cream sandwich',
            calories: 237,
            fat: 9.0,
            carbs: 37,
            protein: 4.3,
            iron: '1%'
          },
          {
            value: false,
            name: 'Eclair',
            calories: 262,
            fat: 16.0,
            carbs: 23,
            protein: 6.0,
            iron: '7%'
          },
          {
            value: false,
            name: 'Cupcake',
            calories: 305,
            fat: 3.7,
            carbs: 67,
            protein: 4.3,
            iron: '8%'
          },
          {
            value: false,
            name: 'Gingerbread',
            calories: 356,
            fat: 16.0,
            carbs: 49,
            protein: 3.9,
            iron: '16%'
          },
          {
            value: false,
            name: 'Jelly bean',
            calories: 375,
            fat: 0.0,
            carbs: 94,
            protein: 0.0,
            iron: '0%'
          },
          {
            value: false,
            name: 'Lollipop',
            calories: 392,
            fat: 0.2,
            carbs: 98,
            protein: 0,
            iron: '2%'
          },
          {
            value: false,
            name: 'Honeycomb',
            calories: 408,
            fat: 3.2,
            carbs: 87,
            protein: 6.5,
            iron: '45%'
          },
          {
            value: false,
            name: 'Donut',
            calories: 452,
            fat: 25.0,
            carbs: 51,
            protein: 4.9,
            iron: '22%'
          },
          {
            value: false,
            name: 'KitKat',
            calories: 518,
            fat: 26.0,
            carbs: 65,
            protein: 7,
            iron: '6%'
          }
        ]
      }),
      props: {
        source: String
      }
    }
  </script>
  
