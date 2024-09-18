<template>
    <div>
      <v-container class="mt-12">
        <v-row>
          <v-col>
            <v-card>
              <v-card-title class="text-h5">แสดงข้อมูลสมาชิก</v-card-title>
              <v-card-text>
                นี่คือหน้าแรกของแอปพลิเคชันของคุณ
              </v-card-text>
              <v-card-actions>
                <v-btn color="primary" @click="$router.push('/datatable')">ดูรายชื่อสมาชิก</v-btn>
              </v-card-actions>
            </v-card>
          </v-col>
        </v-row>
      </v-container>
    </div>
  </template>
  
  <script>
  export default {
    name: 'Home',
  }
  </script>