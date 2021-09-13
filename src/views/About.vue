<template>
  <div>
    <button @click="doBle">Scan: start</button>
    <p>{{ text }}</p>
  </div>
</template>

<script lang="ts">
import { Component, Vue } from "vue-property-decorator";
import BLABLA from "@/types/ble";

@Component
export default class About extends Vue {
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
    this.abl.startScan(
      (a: any) => {
        //if(a.name.toLowerCase().includes("mee")) alert(atob(a.advertisement));

        return atob(a.advertisement).includes("mis") ? this.exit(a) : "";
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
  exit(a: any) {
    this.abl.stopScan(
      () => null,
      () => true
    );
    this.text = new Date().toString();
    this.text += `\n Advertising val: \n${
      atob(a.advertisement).split("mis")[1]
    }`;

    this.abl.connect(
      (status: any) => {
        this.text = JSON.stringify(status);
        this.continue(a);
      },
      (status: any) => {
        this.text = JSON.stringify(status);
        this.abl.reconnect(
          (status: any) => {
            this.text = JSON.stringify(status);
            this.continue(a);
          },
          (status: any) => {
            this.text = JSON.stringify(status);
            this.continue(a);
          },
          { address: a.address }
        );
      },
      { address: a.address }
    );
  }
}
</script>

<style scoped></style>
