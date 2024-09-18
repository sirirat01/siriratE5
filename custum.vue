<template>
  <v-app class="amber accent-1">
    <nav>
      <v-toolbar app class="amber accent-4">
        <v-toolbar-side-icon @click="drawer =!drawer"></v-toolbar-side-icon>
        <v-toolbar-title class="headline">
          <span class="mr-2">Welcome</span>
        </v-toolbar-title>
        <v-spacer></v-spacer>
        <v-btn light flat href="#" target="_blank">
          <v-icon left>home</v-icon>
          <span>หน้าแรก</span>
        </v-btn>
        <v-btn flat href="#" target="_blank">
          <v-icon left>input</v-icon>
          <span>Aboute</span>
        </v-btn>
      </v-toolbar>
      <v-navigation-drawer app class="orange" v-model="drawer">
        <p>รายการ</p>
      </v-navigation-drawer>

      <v-main >
       <NuxtPage></NuxtPage>
        <!-- Main Content -->
      </v-main>
      
    </nav>
  </v-app>
</template>
<script>
export default {
  data() {
    return {
      drawer: true
    };
  }
};
</script>