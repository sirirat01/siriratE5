<template>
  <div>
    <v-container class="mt-12">
      <v-row>
        <v-col>
          <v-card>
            <v-card-title class="text-h5">ยินดีต้อนรับสู่หน้าหลัก</v-card-title>
            <v-card-text>
              เราคือองค์กรที่มุ่งมั่นในการให้บริการที่ดีที่สุดในด้าน [อุตสาหกรรมของคุณ] โดยมีประสบการณ์ยาวนานและทีมงานมืออาชีพที่พร้อมให้คำปรึกษาและบริการที่ครอบคลุมทุกความต้องการของลูกค้า เรามุ่งมั่นในการสร้างความพึงพอใจสูงสุดให้กับลูกค้าของเรา และมีความมุ่งมั่นในการพัฒนาและปรับปรุงการบริการอย่างต่อเนื่อง
            </v-card-text>
            <v-card-actions>
              <!-- <v-btn color="primary" @click="$router.push('/datatable')">ดูรายชื่อสมาชิก</v-btn> -->
            </v-card-actions>
            <v-img
              src="\xampp\htdocs\e5_2567\9.jpg"
              alt="Example Image"
              class="mt-4"
              max-height="800"
              contain
            ></v-img>
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
