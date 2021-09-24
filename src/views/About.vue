<template>
  <div>
    <button @click="doBle">Scan: start</button>
    <p>{{ text }}</p>


  
    <div v-for="(meesha, i) in foundMeeshas" :key="i">
     <div @click="connect(meesha)">
       <h3>{{ i }}</h3> <b>{{ meesha.name }}</b> <i>(id:{{ meesha.ident }})</i> : Signal quality: {{ Math.round(100 + meesha.rssi / 2.55) }} %
     </div> 
    </div>
  </div>
</template>

<script lang="ts">
import { Component, Vue } from "vue-property-decorator";
import BLABLA from "@/types/ble";
 
@Component
export default class About extends Vue {
  private foundMeeshas: any[] = []
  private ble!: BLABLA;
  private abl!: any;
  private text = "";
  a = [{}] as any[];

  mounted() {
    //@ts-ignore
    this.ble = ble;
    //@ts-ignore
    this.abl = bluetoothle;
    this.abl.initialize();
  }

  continue(a: any) {
    this.abl.discover(
      (status: any) => {
        this.text = JSON.stringify(status);
        this.abl.subscribe(
          (status: any) => {
            this.text = atob(status.value ?? "a=======");
          },
          (status: any) => {
            this.text = JSON.stringify(status);
          },
          {
            address: a.address,
            service: "4fafc201-1fb5-459e-8fcc-c5c9c331914b".toUpperCase(),
            characteristic: "f5da3dff-748a-4b4a-9440-11bc5eb73356".toUpperCase(),
          }
        );
      },
      (status: any) => {
        this.text = JSON.stringify(status);
      },
      { address: a.address }
    );
  }
  doBle() {
    this.foundMeeshas = []
    this.abl.startScan(
      (a: any) => {
        //if(a.name.toLowerCase().includes("mee")) alert(atob(a.advertisement));
        if(this.foundMeeshas.filter(b => b.name == a.name && b.advertisement == a.advertisement).length > 0) return;
        
        return atob(a.advertisement).includes("mis") ? this.handleZ(a) : false;
      },
      (a: any) => alert(JSON.stringify(a)),
      {}
    );

    setTimeout(() => {
      this.abl.stopScan(
        () => null,
        () => true
      );
    }, 2000);
  }
  handleZ (a: any) {
    a.ident = atob(a.advertisement).split("mis")[1]
    this.foundMeeshas.push(a)
  }

  connect(a: any) {
    this.abl.disconnect(() => true, () => true, {address: a.address})
    this.abl.close(() => true, () => true, {address: a.address})

    this.abl.connect((a: any) => alert(JSON.stringify(a)), (a: any) => alert(JSON.stringify(a)), {address: a.address})
  }
}
</script>

<style scoped></style>
