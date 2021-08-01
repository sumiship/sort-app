<template>
  <div id="app">
    <div class="array">
      <div
        class="item"
        :style="itemStyle(item)"
        v-for="(item, index) in data"
        :key="index"
      >
        {{ item }}
      </div>
    </div>
    <div>
      <button @click="sort()">sort</button>
      <button @click="shuffle()">shuffle</button>
    </div>
    <div>
      {{ message }}
    </div>
  </div>
</template>

<script lang="ts">
import { Component, Vue } from "vue-property-decorator";
import HelloWorld from "./components/HelloWorld.vue";

@Component({
  components: {
    HelloWorld,
  },
})
export default class App extends Vue {
  private data = [13, 12, 11, 10, 9, 8, 7, 6, 5, 4, 3, 2, 1, 0];
  private message = "";

  private itemStyle(number: number): string {
    return (
      "width:" +
      100 / this.data.length +
      "%; background-color: rgba(226, 141, 43," +
      (1 / this.data.length) * number +
      ");"
    );
  }

  private async sort(): Promise<void> {
    this.message = "";
    for (let i = 0; i < this.data.length; i++) {
      for (let j = 0; j < this.data.length - i; j++) {
        if (this.data[j] > this.data[j + 1])
          await this.swap(j, j + 1, this.data, 180);
        this.data = JSON.parse(JSON.stringify(this.data));
      }
    }
    this.message = "並び替えたよ！";
  }

  private async shuffle(): Promise<void> {
    this.message = "";
    for (let i = 0; i < 100; i++) {
      await this.swap(
        Math.floor(Math.random() * this.data.length),
        Math.floor(Math.random() * this.data.length),
        this.data,
        20
      );
      this.data = JSON.parse(JSON.stringify(this.data));
    }
  }

  private swap(
    id1: number,
    id2: number,
    arr: number[],
    time: number
  ): Promise<number[]> {
    return new Promise(function(resolve) {
      [arr[id1], arr[id2]] = [arr[id2], arr[id1]];
      setTimeout(resolve, time);
    });
  }
}
</script>

<style>
.array {
  display: flex;
  width: 80%;
  margin: 0 auto;
  text-align: center;
  /* background-color: rgb(226, 141, 43); */
}
</style>
