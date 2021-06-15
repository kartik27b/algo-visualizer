<template>
  <m-header>
    <m-button class="pointer-events-none">
      <h1 class="text-4xl font-extrabold">
        Sorting Algorithms Visualiser
      </h1>
    </m-button>
    <span class="mr-2"></span>
    <m-button @click="shuffle">Shuffle</m-button>
    <m-button @click="selectionSort">Selection Sort</m-button>
    <m-button @click="bubbleSort">Bubble Sort</m-button>
    <m-button @click="mergeSort">Merge Sort</m-button>
    <m-button @click="quickSort">Quick Sort</m-button>
    <m-button @click="insertionSort">Insertion Sort</m-button>
  </m-header>
  <!-- <div class="flex-1"> -->
  <div class="mx-auto w-4/5 h-4/5 flex p-2 items-end justify-around">
    <transition-group :name="showAnimation ? 'flip-list' : ''">
      <div
        v-for="item in arr"
        :key="item.id"
        class="rounded-md  w-4 mx-1 "
        :class="{
          mytransition: !showAnimation,
          'bg-gray-700': !item.active,
          'bg-yellow-500': item.active,
        }"
        :style="barStyle(item.value)"
      ></div>
    </transition-group>
  </div>
  <!-- </div> -->
</template>

<script>
import { v4 } from "uuid";
import MButton from "../components/MButton.vue";
/* eslint-disable no-unused-vars */
import _ from "lodash";
import MHeader from "../components/MHeader.vue";

function getRandomInt(min, max) {
  min = Math.ceil(min);
  max = Math.floor(max);
  return Math.random() * (max - min + 1) + min;
}

export default {
  name: "Sorting",
  components: { MButton, MHeader },
  data() {
    const arr = [];
    for (let i = 0; i < 50; i++) {
      arr.push({ id: v4(), value: getRandomInt(1, 10), active: false });
    }
    return {
      arr: arr,
      animations: [],
      showAnimation: false,
    };
  },
  methods: {
    disableAnimation: async function() {
      this.showAnimation = false;
      for (let i = 0; i < this.arr.length; i++) {
        this.arr[i].active = false;
      }
    },
    barStyle(height) {
      return {
        height: height * 3 + "rem",
      };
    },
    sleep() {
      return new Promise((resolve) => setTimeout(resolve, 50));
    },
    shuffle() {
      this.showAnimation = true;
      this.arr = _.shuffle(this.arr);
      console.log(this.arr);
    },
    merge: async function(l, mid, r) {
      const L = [];
      const R = [];
      const n1 = mid - l + 1;
      const n2 = r - mid;
      for (let i = 0; i < n1; i++) {
        L.push(this.arr[l + i].value);
      }

      for (let i = 0; i < n2; i++) {
        R.push(this.arr[mid + 1 + i].value);
      }

      let i = 0,
        j = 0,
        k = l;
      while (i < n1 && j < n2) {
        this.arr[k].active = true;
        await this.sleep();
        if (L[i] <= R[j]) {
          this.arr[k].value = L[i];
          i++;
        } else {
          this.arr[k].value = R[j];
          j++;
        }
        k++;
      }
      while (i < n1) {
        this.arr[k].active = true;
        await this.sleep();
        this.arr[k].value = L[i];
        i++;
        k++;
      }
      while (j < n2) {
        this.arr[k].active = true;
        await this.sleep();
        this.arr[k].value = R[j];
        j++;
        k++;
      }
    },
    mergeSortHelper: async function(l, r) {
      if (l >= r) {
        return;
      }
      let mid = (l + r) / 2;
      mid = Math.floor(mid);
      await this.mergeSortHelper(l, mid);
      await this.mergeSortHelper(mid + 1, r);
      await this.merge(l, mid, r);
    },
    mergeSort: async function() {
      await this.disableAnimation();
      console.log("Merge sort sort");
      this.mergeSortHelper(0, this.arr.length - 1);
      console.log("done");
    },
    partition: async function(l, r) {
      const pivot = this.arr[r].value;
      this.arr[r].active = true;
      await this.sleep();

      let i = l - 1;

      for (let j = l; j <= r - 1; j++) {
        this.arr[j].active = true;
        await this.sleep();
        if (this.arr[j].value < pivot) {
          i++;

          const temp = this.arr[i].value;
          this.arr[i].value = this.arr[j].value;
          this.arr[j].value = temp;
        }
        this.arr[j].active = false;
        await this.sleep();
      }
      const curr = this.arr[i + 1].value;
      this.arr[i + 1].value = this.arr[r].value;
      this.arr[r].value = curr;
      return i + 1;
    },
    quickSortHelper: async function(l, r) {
      if (l < r) {
        const pos = await this.partition(l, r);

        await this.quickSortHelper(l, pos - 1);
        await this.quickSortHelper(pos + 1, r);
      }
    },
    quickSort: async function() {
      await this.disableAnimation();
      await this.quickSortHelper(0, this.arr.length - 1);
      for (let i = 0; i < this.arr.length; i++) {
        this.arr[i].active = true;
        await this.sleep();
      }
    },
    selectionSort: async function() {
      await this.disableAnimation();
      console.log("Selection sort");
      let i, j, idx;
      let n = this.arr.length;

      for (i = 0; i < n; i++) {
        idx = i;

        for (j = i + 1; j < n; j++) {
          this.arr[j].active = true;
          if (this.arr[j].value < this.arr[idx].value) {
            idx = j;
          }
          await this.sleep();
          this.arr[j].active = false;
        }

        const val = this.arr[i].value;
        this.arr[i].value = this.arr[idx].value;
        this.arr[idx].value = val;
        this.arr[i].active = true;
        await this.sleep();
      }
      console.log("done");
    },
    bubbleSort: async function() {
      await this.disableAnimation();
      console.log("bubble sort");
      let i, j;
      const n = this.arr.length;

      for (i = 0; i < n; i++) {
        for (j = 0; j < n - i - 1; j++) {
          this.arr[j].active = true;
          await this.sleep();
          this.arr[j + 1].active = true;

          if (this.arr[j].value > this.arr[j + 1].value) {
            const temp = this.arr[j].value;
            this.arr[j].value = this.arr[j + 1].value;
            this.arr[j + 1].value = temp;
          }
          this.arr[j].active = false;
          this.arr[j + 1].active = false;
        }

        this.arr[n - i - 1].active = true;
      }
    },
    insertionSort: async function() {
      await this.disableAnimation();
      let i, key, j;
      for (i = 1; i < this.arr.length; i++) {
        const key = this.arr[i].value;
        if (i + 1 <= this.arr.length - 1) {
          this.arr[i + 1].active = true;
          await this.sleep();
        }
        j = i - 1;

        while (j >= 0 && this.arr[j].value > key) {
          this.arr[j + 1].active = true;
          this.arr[j].active = true;
          await this.sleep();
          this.arr[j + 1].value = this.arr[j].value;

          this.arr[j + 1].active = false;
          this.arr[j].active = false;
          await this.sleep();
          j = j - 1;
        }
        this.arr[j + 1].value = key;
      }
    },
  },
};
</script>

<style scoped>
.flip-list-move {
  transition: transform 1s ease;
}

.mytransition {
  transition: height, background-color 45ms ease;
}
</style>
