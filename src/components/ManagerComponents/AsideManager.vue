<template>
  <v-navigation-drawer dark permanent expand-on-hover absolute>
    <v-list>
      
      <v-divider></v-divider>
      <v-list-item-title class="secondary">
        <!-- Icon manage -->
        <v-icon class="purple--text">mdi-account-box</v-icon>
        My profil
      </v-list-item-title>
      <v-divider></v-divider>

      <!-- Admin info -->
      <v-list-item link :to="{name: 'newpass'}">
        <v-list-item-content>
          <v-list-item-title class="title">{{loggedUserInfo.firstName + ' ' + loggedUserInfo.lastName }}</v-list-item-title>
          <v-list-item-subtitle>{{loggedUserInfo.mail}}</v-list-item-subtitle>
        </v-list-item-content>
      </v-list-item>
    </v-list>

    <v-divider></v-divider>
      <!-- Title Manage -->
    <v-list-item-title class="secondary white--text">
        <!-- Icon manage -->
        <v-icon class="success--text">mdi-plus-thick</v-icon>
        Manage
    </v-list-item-title>
    <v-divider></v-divider>

    <v-list nav dense>
      <!-- New user -->
      <v-list-item link @click="emitNewUserCreator()">
        <v-list-item-title>New User</v-list-item-title>
      </v-list-item>

      <!-- New promo -->
      <v-list-item link @click="emitNewPromoCreator()">
        <v-list-item-title>New Promo</v-list-item-title>
      </v-list-item>

      <!-- Promos -->
      <v-divider></v-divider>
      <v-list-item-title class="secondary">
        <v-icon class="blue--text darken-4">mdi-account-group</v-icon>
        Promos
      </v-list-item-title>
      <v-divider></v-divider>
      <v-list-item link v-for="promotion in promos" :key="promotion.name" @click="getSelectedPromo(promotion.name)">
        <v-list-item-title>{{ promotion.name }}</v-list-item-title>
      </v-list-item>

      <!-- Users -->
      <v-divider></v-divider>
      <v-list-item-title class="secondary" v-if="selectedPromo">
        <v-icon class="teal--text">mdi-account</v-icon>
        {{selectedPromo}}
      </v-list-item-title>
      <v-divider></v-divider>
      
      <div v-for="user in users" :key="user.firstName">
        <v-list-item link v-if="user.namePromo === selectedPromo" @click="getSelectedUser(user), emitSelectedUser()">
          <v-list-item-title>{{ user.firstName + ' ' + user.lastName }}</v-list-item-title>
        </v-list-item>
      </div>
    </v-list>
  </v-navigation-drawer>
</template>

<script>
import { mapState } from "vuex";
  export default {
    props: ["promos", "users"],
    data() {
      return {
        title: "Manager",
        selectedPromo: "",
        selectedUser: "",
        booleans: [{
            newUser: false
          },
          {
            newPromo: false
          }
        ]
      };
    },
    computed: {
      ...mapState({
        loggedUserInfo: "user"
      })
    },
    methods: {
      // Get selected promo name and add to the selectedPromo data string
      getSelectedPromo(promotionName) {
        this.selectedPromo = promotionName;
      },
      // Get selected user object and add to the selectedUser data object
      getSelectedUser(userObject) {
        this.selectedUser = userObject;
      },
      // Emit selected user object to the userSelector method in Manager
      emitSelectedUser() {
        if (typeof this.selectedUser === 'string')
          this.$emit("userSelector", this.selectedUser = "");
        else
          this.$emit("userSelector", this.selectedUser);
        this.selectedUser = "";
      },
      emitNewUserCreator() {
        if (this.booleans[0].newUser)
          this.$emit("newUserCreator", (this.booleans[0].newUser = false));
        else
          this.$emit("newUserCreator", (this.booleans[0].newUser = true));
      },
      emitNewPromoCreator() {
        if (this.booleans[1].newPromo)
          this.$emit("newPromoCreator", (this.booleans[1].newPromo = false));
        else
          this.$emit("newPromoCreator", (this.booleans[1].newPromo = true));
      }
    }
  };
</script>
