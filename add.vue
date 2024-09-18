<template>
  <v-container
    fluid
    fill-height
    class="d-flex align-center justify-center pa-4"
  >
    <v-card
      class="pa-4"
      max-width="800"
      elevation="8"
      rounded="lg"
    >
      <v-card-title class="text-center">
        <h1 :title="id">{{ msg }}</h1>
      </v-card-title>

      <v-card-subtitle class="text-center">
        <h1>{{ state !== null ? `State: ${state}` : status }}</h1>
      </v-card-subtitle>

      <v-divider></v-divider>

      <v-card-text class="d-flex justify-around flex-wrap">
        <div class="text-center graph-container">
          <h1>Temp</h1>
          <v-progress-circular
            :model-value="tempValue"
            :rotate="360"
            :size="200"
            :width="30" 
            color="teal"
          >
            <template v-slot:default> {{ tempValue }} °C </template>
          </v-progress-circular>
        </div>

        <div class="text-center graph-container">
          <h1>Humi</h1>
          <v-progress-circular
            :model-value="humiValue"
            :rotate="360"
            :size="200" 
            :width="30" 
            color="red"
          >
            <template v-slot:default> {{ humiValue }} % </template>
          </v-progress-circular>
        </div>
      </v-card-text>

      <v-divider></v-divider>

      <!-- ปุ่ม Heater, Dehumidifier, Status -->
      <v-card-actions class="d-flex justify-around">
        <v-btn
          :color="heaterOn ? 'red' : 'grey'"
          @click="toggleHeater"
        >
          Heater: {{ heaterOn ? 'On' : 'Off' }}
        </v-btn>

        <v-btn
          :color="dehumidifierOn ? 'blue' : 'grey'"
          @click="toggleDehumidifier"
        >
          Dehumidifier: {{ dehumidifierOn ? 'On' : 'Off' }}
        </v-btn>

        <v-btn
          color="green"
          disabled
        >
          Status: On
        </v-btn>
      </v-card-actions>

      <v-card-actions class="d-flex flex-column align-center">
        <v-alert
          v-if="alertMessages.length"
          v-for="(alert, index) in alertMessages"
          :key="index"
          :type="alert.type"
          class="ma-3"
        >
          {{ alert.message }}
        </v-alert>
      </v-card-actions>
    </v-card>
  </v-container>
</template>

<script>
import mqtt from "mqtt";

export default {
  data() {
    return {
      id: 10,
      msg: "",
      status: "กำลังเชื่อมต่อ MQTT...", // ข้อความเริ่มต้น
      tempValue: 25.40,
      humiValue: 79.70,
      state: null, // ค่าเริ่มต้นของ state
      tempLow: 26.00,
      tempHigh: 29.00,
      humiLow: 80.25,
      humiHigh: 90.10,
      alertMessages: [],
      heaterOn: false, // สถานะปุ่ม Heater
      dehumidifierOn: false, // สถานะปุ่ม Dehumidifier
    };
  },

  created() {
    this.client = mqtt.connect("ws://broker.emqx.io:8083/mqtt");
    this.client.on("connect", this.onMqttConnect);
    this.client.on("message", this.onMqttMessage);
    this.client.on("reconnect", this.handleOnReConnect);
  },

  beforeDestroy() {
    this.client && this.client.end();
  },

  methods: {
    onMqttConnect() {
      this.status = "MQTT connected";
      this.client.subscribe("status");
      this.client.subscribe("room/sensor");
    },

    onMqttMessage(topic, message) {
      if (topic === "status") {
        this.msg = message.toString();
      }
      if (topic === "room/sensor") {
        try {
          const data = JSON.parse(message.toString());
          console.log('Received data:', data); // Debugging: Log received data
          
          // อัปเดตค่าที่ได้รับจาก MQTT
          this.tempValue = data.temp !== undefined ? parseFloat(data.temp) : this.tempValue;
          this.humiValue = data.humi !== undefined ? parseFloat(data.humi) : this.humiValue;

          // อัปเดตค่า state และค่าควบคุม
          this.state = data.state !== undefined ? data.state : this.state;
          this.tempLow = data.t1 !== undefined ? parseFloat(data.t1) : this.tempLow;
          this.tempHigh = data.t2 !== undefined ? parseFloat(data.t2) : this.tempHigh;
          this.humiLow = data.h1 !== undefined ? parseFloat(data.h1) : this.humiLow;
          this.humiHigh = data.h2 !== undefined ? parseFloat(data.h2) : this.humiHigh;

          this.checkAlerts();  // อัปเดตการแจ้งเตือน
          this.updateControls(); // อัปเดตสถานะของปุ่มควบคุม
        } catch (e) {
          console.error('Failed to parse message:', e);
        }
      }
    },

    handleOnReConnect() {
      this.status = "Reconnecting...";
    },

    checkAlerts() {
      this.alertMessages = [];

      // ตรวจสอบการแจ้งเตือนโดยใช้ค่าที่กำหนดใน data
      if (this.tempValue > this.tempHigh) {
        this.alertMessages.push({
          message: `อุณหภูมิสูงเกินไป: ${this.tempValue} °C`,
          type: "error"
        });
      } else if (this.tempValue < this.tempLow) {
        this.alertMessages.push({
          message: `อุณหภูมิต่ำเกินไป: ${this.tempValue} °C`,
          type: "warning"
        });
      } else {
        this.alertMessages.push({
          message: `อุณหภูมิปกติ: ${this.tempValue} °C`,
          type: "info"
        });
      }

      if (this.humiValue > this.humiHigh) {
        this.alertMessages.push({
          message: `ความชื้นสูงเกินไป: ${this.humiValue} %`,
          type: "error"
        });
      } else if (this.humiValue < this.humiLow) {
        this.alertMessages.push({
          message: `ความชื้นต่ำเกินไป: ${this.humiValue} %`,
          type: "warning"
        });
      } else {
        this.alertMessages.push({
          message: `ความชื้นปกติ: ${this.humiValue} %`,
          type: "info"
        });
      }
    },

    // อัปเดตสถานะของปุ่ม Heater และ Dehumidifier
    updateControls() {
      this.heaterOn = this.tempValue < this.tempLow;
      this.dehumidifierOn = this.humiValue < this.humiLow;
    },

    async updateValues() {
      const payload = JSON.stringify({
        temp: this.tempValue,
        humi: this.humiValue,
        state: this.state,
        t1: this.tempLow,
        t2: this.tempHigh,
        h1: this.humiLow,
        h2: this.humiHigh
      });
      this.client.publish("room/sensor", payload);
      this.checkAlerts();
    }
  },

  watch: {
    tempValue(newVal) {
      this.updateValues();
    },
    humiValue(newVal) {
      this.updateValues();
    }
  }
};
</script>

<style scoped>
.v-card {
  background: #f5f5f5;
}

.v-card-title h1,
.v-card-subtitle h1 {
  margin: 0;
  font-size: 28px; /* เพิ่มขนาดฟอนต์ให้ใหญ่ขึ้น */
  color: #333;
  font-weight: bold; /* ทำให้ตัวหนาขึ้น */
}

.v-card-text h1 {
  margin: 20px 0;
  font-size: 22px; /* เพิ่มขนาดฟอนต์ให้ใหญ่ขึ้น */
}

.v-btn {
  font-weight: bold; /* ทำให้ตัวหนาขึ้น */
  font-size: 18px; /* เพิ่มขนาดฟอนต์ให้ใหญ่ขึ้น */
  padding: 10px 20px; /* เพิ่มระยะห่างภายในปุ่มให้ใหญ่ขึ้น */
}

.graph-container {
  flex: 1;
  display: flex;
  justify-content: center;
  align-items: center;
  max-width: 300px; /* ขนาดสูงสุดของกราฟ */
  width: 100%;
}

.v-progress-circular {
  width: 100% !important; /* ให้กราฟมีขนาดเต็มที่ */
  max-width: 300px; /* ขนาดสูงสุดของกราฟ */
  height: auto;
}

.v-divider {
  margin: 20px 0;
}

.v-alert {
  font-size: 18px; /* เพิ่มขนาดฟอนต์ให้ใหญ่ขึ้น */
  padding: 15px;
}
</style>
