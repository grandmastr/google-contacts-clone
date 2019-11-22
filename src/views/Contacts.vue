<template>
  <v-app id="inspire">
    <v-navigation-drawer
      v-model="drawer"
      :clipped="$vuetify.breakpoint.lgAndUp"
      app
    >
      <v-list dense>
        <v-btn
          style="margin-left: 15px; margin-bottom: 20px;"
          min-height="40"
          min-width="50%"
          rounded
          @click="dialog = !dialog"
        >
          <v-icon size="30">mdi-plus</v-icon>
          <span style="text-transform: none;">Create Contact</span>
        </v-btn>
        <template v-for="item in items">
          <v-row v-if="item.heading" :key="item.heading" align="center">
            <v-col cols="6">
              <v-subheader v-if="item.heading">
                {{ item.heading }}
              </v-subheader>
            </v-col>
            <v-col cols="6" class="text-center">
              <a href="#" class="body-2 black--text">EDIT</a>
            </v-col>
          </v-row>
          <v-list-group
            v-else-if="item.children"
            :key="item.text"
            v-model="item.model"
            :prepend-icon="item.model ? item.icon : item['icon-alt']"
            append-icon=""
          >
            <template v-slot:activator>
              <v-list-item>
                <v-list-item-content>
                  <v-list-item-title>
                    {{ item.text }}
                  </v-list-item-title>
                </v-list-item-content>
              </v-list-item>
            </template>
            <v-list-item v-for="(child, i) in item.children" :key="i" link>
              <v-list-item-action v-if="child.icon">
                <v-icon>{{ child.icon }}</v-icon>
              </v-list-item-action>
              <v-list-item-content>
                <v-list-item-title>
                  {{ child.text }}
                </v-list-item-title>
              </v-list-item-content>
            </v-list-item>
          </v-list-group>
          <v-list-item v-else :key="item.text" link>
            <v-list-item-action>
              <v-icon>{{ item.icon }}</v-icon>
            </v-list-item-action>
            <v-list-item-content>
              <v-list-item-title>
                {{ item.text }}
              </v-list-item-title>
            </v-list-item-content>
          </v-list-item>
        </template>
      </v-list>
    </v-navigation-drawer>

    <v-app-bar
      :clipped-left="$vuetify.breakpoint.lgAndUp"
      app
      color="white"
      dark
      flat
    >
      <v-app-bar-nav-icon
        style="color: #5F6368"
        @click.stop="drawer = !drawer"
      />
      <v-toolbar-title style="width: 300px" class="ml-0 pl-4">
        <v-avatar size="40px" class="mx-3">
          <v-btn
            fab
            small
            style="margin-top: -1px; background-color: #2255E8"
            elevation="0"
            aria-disabled="true"
            :ripple="false"
          >
            <v-icon style="color: #ffffff">mdi-account</v-icon>
          </v-btn>
        </v-avatar>
        <span class="hidden-sm-and-down" style="color: #5F6368">Contacts</span>
      </v-toolbar-title>
      <v-text-field
        solo-inverted=""
        prepend-inner-icon="mdi-magnify"
        label="Search"
        style="background-color: #F1F3F4"
        class="hidden-sm-and-down"
        hide-details
        color="#5F6368"
        :flat="!focused"
        light
        @focus="searchFocused"
        @focusout="searchFocused"
      />
      <v-spacer />
      <v-btn icon>
        <v-icon color="#5F6368">mdi-apps</v-icon>
      </v-btn>
      <v-btn icon>
        <v-icon color="#5F6368">mdi-bell</v-icon>
      </v-btn>
      <v-btn icon>
        <v-avatar size="40px" class="mx-3">
          <v-btn
            fab
            small
            :style="
              `background-color: ${
                colors[Math.floor(Math.random() * colors.length)]
              }; font-weight: bold`
            "
            elevation="0"
            aria-disabled="true"
            :ripple="false"
          >
            A
          </v-btn>
        </v-avatar>
      </v-btn>
    </v-app-bar>
    <v-content>
      <v-container class="fill-height" fluid>
        <v-simple-table fixed-header style="width: 100%">
          <template v-slot:default>
            <thead>
              <tr>
                <th class="text-left">Name</th>
                <th class="text-left">Email</th>
                <th class="text-left">Phone Number</th>
                <th class="text-left">Job Title & company</th>
                <th class="text-left">
                  <v-btn text fab small>
                    <v-icon>mdi-dots-vertical</v-icon>
                  </v-btn>
                </th>
              </tr>
            </thead>
            <p style="font-size: 0.8em; margin: 10px 0; padding-left: 10px;">
              CONTACTS ({{ contacts.length }})
            </p>
            <tbody>
              <tr v-for="(contact, i) in contacts" :key="i" @mouseover="hide">
                <td>
                  <v-btn
                    class="hidden-sm-and-down"
                    fab
                    small
                    elevation="0"
                    :style="
                      `background-color: ${
                        colors[Math.floor(Math.random() * colors.length)]
                      }; color: #ffffff; margin-right: 10px; font-weight: bold; text-transform: none`
                    "
                  >
                    {{
                      contact.name.length === 2
                        ? contact.name
                            .split('')
                            .slice(0, 2)
                            .join('')
                        : contact.name.toUpperCase()[0]
                    }}
                  </v-btn>
                  {{ contact.name }}
                </td>
                <td>{{ contact.email || '' }}</td>
                <td>{{ contact.phone || '' }}</td>
                <td>{{ contact.title || '' }}</td>
                <td>
                  <v-btn text fab small>
                    <v-icon>mdi-dots-vertical</v-icon>
                  </v-btn>
                </td>
              </tr>
            </tbody>
          </template>
        </v-simple-table>
      </v-container>
    </v-content>
    <v-dialog v-model="dialog" max-width="600px">
      <v-card>
        <v-card-title style="font-size: 1em">
          Create new contact
        </v-card-title>
        <v-container class="hidden-sm-and-down">
          <v-row>
            <v-avatar
              style="margin-top: 30px;"
              size="40px"
              cols="12"
              sm="2"
              class="mx-3"
            >
              <img
                src="//ssl.gstatic.com/s2/oz/images/sge/grey_silhouette.png"
                alt=""
              />
            </v-avatar>
            <v-col cols="12" sm="5">
              <v-text-field label="First name"></v-text-field>
            </v-col>
            <v-col cols="12" sm="5">
              <v-text-field label="Last name"></v-text-field>
            </v-col>
          </v-row>
          <v-row style="margin-top: -30px;">
            <v-avatar
              style="margin-top: 18px;"
              size="50px"
              cols="12"
              sm="2"
              class="mx-2"
            >
              <v-icon class="hidden-sm-and-down">mdi-domain</v-icon>
            </v-avatar>
            <v-col cols="12" sm="5">
              <v-text-field label="Company"></v-text-field>
            </v-col>
            <v-col cols="12" sm="5">
              <v-text-field label="Job title"></v-text-field>
            </v-col>
          </v-row>
          <v-row style="margin-top: -30px;">
            <v-avatar
              style="margin-top: 18px;"
              size="50px"
              cols="12"
              sm="2"
              class="mx-2"
            >
              <v-icon class="hidden-sm-and-down">mdi-email-outline</v-icon>
            </v-avatar>
            <v-col cols="12" sm="10">
              <v-text-field label="Email"></v-text-field>
            </v-col>
          </v-row>
          <v-row style="margin-top: -30px;">
            <v-avatar
              style="margin-top: 18px;"
              size="50px"
              cols="12"
              sm="2"
              class="mx-2"
            >
              <v-icon class="hidden-sm-and-down">mdi-phone-outline</v-icon>
            </v-avatar>
            <v-col cols="12" sm="10">
              <v-text-field label="Phone"></v-text-field>
            </v-col>
          </v-row>
          <v-row style="margin-top: -30px;">
            <v-avatar
              style="margin-top: 18px;"
              size="50px"
              cols="12"
              sm="2"
              class="mx-2"
            >
              <v-icon class="hidden-sm-and-down">mdi-note-outline</v-icon>
            </v-avatar>
            <v-col cols="12" sm="10">
              <v-text-field label="Notes"></v-text-field>
            </v-col>
          </v-row>
        </v-container>
        <v-container class="hidden-md-and-up">
          <v-row style="margin-top: -30px;">
            <v-col cols="12" sm="12">
              <v-text-field label="First name"></v-text-field>
            </v-col>
            <v-col cols="12" sm="12">
              <v-text-field label="Last name"></v-text-field>
            </v-col>
            <v-col cols="12" sm="12">
              <v-text-field label="Company"></v-text-field>
            </v-col>
            <v-col cols="12" sm="12">
              <v-text-field label="Job title"></v-text-field>
            </v-col>
            <v-col cols="12" sm="12">
              <v-text-field label="Email"></v-text-field>
            </v-col>
            <v-col cols="12" sm="12">
              <v-text-field label="Phone"></v-text-field>
            </v-col>
            <v-col cols="12" sm="12">
              <v-text-field label="Notes"></v-text-field>
            </v-col>
          </v-row>
        </v-container>
        <v-card-actions>
          <v-btn text color="primary" style="text-transform: none"
            >Show More</v-btn
          >
          <v-spacer />
          <v-btn
            text
            color="primary"
            @click="dialog = false"
            style="text-transform: none"
            >Cancel</v-btn
          >
          <v-btn
            color="primary"
            text
            @click="dialog = false"
            style="text-transform: none"
            >Save</v-btn
          >
        </v-card-actions>
      </v-card>
    </v-dialog>
  </v-app>
</template>

<script>
import contacts from '../assets/contacts';

export default {
  data: () => ({
    focused: false,
    dialog: false,
    drawer: null,
    colors: ['#52AD41', '#7C009C', '#405B5E', '#CB0000', '#603F36', '#87009F'],
    items: [
      {
        icon: 'mdi-account-outline',
        text: `Contacts (${contacts.details.length})`
      },
      { icon: 'mdi-history', text: 'Frequently contacted' },
      { icon: 'mdi-content-copy', text: 'Duplicates' },
      {
        icon: 'mdi-chevron-up',
        'icon-alt': 'mdi-chevron-down',
        text: 'Labels',
        model: true,
        children: [{ icon: 'mdi-plus', text: 'Create label' }]
      },
      {
        icon: 'mdi-chevron-up',
        'icon-alt': 'mdi-chevron-down',
        text: 'More',
        model: false,
        children: [
          { text: 'Import' },
          { text: 'Export' },
          { text: 'Print' },
          { text: 'Undo changes' },
          { text: 'Other contacts' }
        ]
      }
    ]
  }),
  methods: {
    searchFocused() {
      this.focused = !this.focused;
    },
    hide(e) {
      // eslint-disable-next-line no-console
      console.log(e);
    }
  },
  computed: {
    contacts: () => contacts.details
  }
};
</script>
