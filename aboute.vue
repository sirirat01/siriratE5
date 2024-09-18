<template>
  <v-container
    fluid
    fill-height
    class="d-flex align-center justify-center pa-4"
  >
    <v-card
      class="mx-auto pa-8"
      max-width="600"
      elevation="8"
      rounded="lg"
    >
      <!-- Image positioned at the top -->
      <v-img
        class="mx-auto mb-4"
        src="\xampp\htdocs\e5_2567\7.jpg"
        max-width="150"
        contain
      ></v-img>

      <v-card-title class="text-center">
        <h1>ศิริรัตน์ คัชเขียว</h1>
      </v-card-title>

      <v-card-subtitle class="text-center">
        <h2>ยินดีต้อนรับสู่เพจของเรา</h2>
      </v-card-subtitle>

      <v-card-text>
        <p>
          "การออกแบบเว็บไซต์ที่ดีต้องคำนึงถึงความสะดวกสบายของผู้ใช้งานเป็นหลัก ซึ่งรวมถึงการจัดเรียงข้อมูล การเลือกใช้สี และการจัดวางองค์ประกอบต่างๆ ให้น่าสนใจและเข้ากันได้ดี การทดสอบและปรับปรุงอยู่เสมอเป็นสิ่งสำคัญเพื่อให้แน่ใจว่าเว็บไซต์ของคุณทำงานได้ดีและตอบสนองความต้องการของผู้ใช้ได้อย่างเต็มที่"
        </p>
        <p>
          "เมื่อสร้างเนื้อหาบนเว็บไซต์, ควรคำนึงถึงการจัดเรียงเนื้อหาให้อ่านง่ายและให้ข้อมูลที่จำเป็นสำหรับผู้ใช้ ด้วยการออกแบบที่สะอาดตาและง่ายต่อการเข้าถึงข้อมูล เนื้อหาที่ดีจะช่วยเพิ่มประสิทธิภาพของเว็บไซต์และสร้างประสบการณ์ที่ดีให้กับผู้ใช้"
        </p>
      </v-card-text>

      <v-card-actions>
        <v-btn
          class="mx-auto"
          color="primary"
          @click="goHome"
        >
          กลับไปที่หน้าหลัก
        </v-btn>
      </v-card-actions>
    </v-card>
  </v-container>
</template>

<script>
export default {
  methods: {
    goHome() {
      this.$router.push('/home');
    }
  }
}
</script>

<style scoped>
.v-card {
  background: #f5f5f5;
}

.v-card-title {
  border-bottom: 1px solid #ddd;
}

.v-card-subtitle {
  margin-bottom: 16px;
  color: #555;
}

.v-card-text {
  line-height: 1.6;
}

.v-btn {
  font-weight: 500;
}
</style>
