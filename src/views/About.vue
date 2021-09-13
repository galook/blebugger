<template>
  <div>
    <button @click="doBle">CLICK</button>
  </div>
</template>

<script lang="ts">
import { Component, Vue } from "vue-property-decorator";
import BLABLA from "@/types/ble";

@Component
export default class About extends Vue {
  private ble!: BLABLA;
  private abl!: any;
  a = [{}] as any[];

  mounted() {
    //@ts-ignore
    this.ble = ble;
    //@ts-ignore
    this.abl = bluetoothle;
    this.abl.initialize();
  }
  doBle() {
 
    this.abl.startScan(
      (a: any) => (a.name.includes("ESP") ? this.exit(a) : ""),
      (a: any) => alert(JSON.stringify(a)),
      {}
    );

    // this.ble.scan(
    //   [],
    //   7,
    //   (data: any) => (this.a += JSON.stringify(data)),
    //   (data: any) => (this.a += JSON.stringify(data))
    // );
  }
  exit(a: any) {
    this.abl.stopScan(() => null, () => true)
    alert(atob(a.advertisement))
  }
}
</script>

<style scoped>
</style>
