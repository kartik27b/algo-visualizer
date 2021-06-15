<template>
  <m-header>
    <m-button class="pointer-events-none">
      <h1 class="text-4xl font-extrabold">
        Graphs Algorithms Visualiser
      </h1>
    </m-button>
    <span class="mr-2"></span>
    <m-button @click="bfs">BFS</m-button>
    <m-button @click="dfs">DFS</m-button>
    <m-button @click="disjktra">Disjktra</m-button>
  </m-header>
  <div class="flex-1 p-4 bg-white">
    <!-- <table> -->
    <table class="table-fixed " cellspacing="1" cellpadding="0">
      <tbody @mousedown="mousePressed = true" @mouseup="mousePressed = false">
        <tr v-for="(row, rowIdx) in grid" :key="rowIdx">
          <td v-for="(col, colIdx) in row" :key="colIdx">
            <div
              class="w-10 h-10 border border-gray-400 hover:bg-yellow-400 "
              :class="{
                'bg-yellow-400': col.wall,
                'bg-green-500': col.dest,
                'bg-blue-500': col.source,
                'bg-blue-400': col.visited,
                'bg-purple-600': col.showPath,
              }"
              @mouseover="setActive(rowIdx, colIdx)"
            ></div>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
/* eslint-disable no-unused-vars */
/* eslint-disable no-constant-condition */
import PriorityQueue from "../utils";
import MHeader from "../components/MHeader.vue";
import MButton from "../components/MButton.vue";
import { v4 } from "uuid";
const X = [0, 1, 0, -1];
const Y = [-1, 0, 1, 0];
// console.log(PriorityQueue);
function getRandomInt(min, max) {
  min = Math.ceil(min);
  max = Math.floor(max);
  return Math.random() * (max - min + 1) + min;
}

const isvalid = (i, j, m, n) => {
  return i >= 0 && i < m && j >= 0 && j < n;
};

export default {
  name: "Graphs",
  components: {
    MHeader,
    MButton,
  },
  data() {
    const localGrid = Array.from(Array(14), () => new Array(35));
    for (let i = 0; i < localGrid.length; i++) {
      for (let j = 0; j < localGrid[i].length; j++) {
        localGrid[i][j] = {
          visited: false,
          wall: false,
          source: false,
          dest: false,
          showPath: false,
          weight: getRandomInt(10, 100),
          row: i,
          col: j,
          parent: { row: -1, col: -1 },
        };
      }
    }
    localGrid[3][26].dest = true;
    localGrid[5][12].source = true;

    return {
      grid: localGrid,
      mousePressed: false,
      src: null,
      dest: null,
    };
  },
  methods: {
    setActive(rowIdx, colIdx) {
      const curr = this.grid[rowIdx][colIdx];
      if (this.mousePressed && !curr.source && !curr.dest) {
        this.grid[rowIdx][colIdx].wall = true;
      }
    },
    sleep() {
      return new Promise((resolve) => setTimeout(resolve, 10));
    },
    minDistance() {
      // int minDistance(int dist[], bool sptSet[])
      // {
      //     // Initialize min value
      //     int min = INT_MAX, min_index;
      //     for (int v = 0; v < V; v++)
      //         if (sptSet[v] == false && dist[v] <= min)
      //             min = dist[v], min_index = v;
      //     return min_index;
      // }
    },
    disjktra: async function() {
      // const src = this.grid[5][12];
      // const dest = this.grid[3][26];
      // const rows = this.grid.length;
      // const cols = this.grid[0].length;
      // src.weight = 0;
      // const pq = new PriorityQueue((a, b) => a.weight < b.weight);
      // pq.push(src);
      // while (!pq.isEmpty()) {
      //   const top = pq.pop();
      //   // if (top.dest) {
      //   //   break;
      //   // }
      //   for (let k = 0; k < 4; k++) {
      //     const dx = top.row + X[k];
      //     const dy = top.col + Y[k];
      //     if (!isvalid(dx, dy, rows, cols)) {
      //       continue;
      //     }
      //     const curr = this.grid[dx][dy];
      //     if (curr.wall) {
      //       continue;
      //     }
      //     curr.visited = true;
      //     if (curr.weight > top.weight + 1) {
      //       curr.weight = top.weight + 1;
      //       // curr.visited = true;
      //       await this.sleep();
      //       curr.parent.row = top.row;
      //       curr.parent.col = top.col;
      //       pq.push(curr);
      //     }
      //   }
      // }
      // let p = dest;
      // while (true) {
      //   const { row, col } = p.parent;
      //   if (row === -1 && col === -1) {
      //     break;
      //   }
      //   console.log(p);
      //   p.visited = false;
      //   p.wall = false;
      //   p.source = false;
      //   p.dest = false;
      //   p.showPath = true;
      //   await this.sleep();
      //   await this.sleep();
      //   await this.sleep();
      //   p = this.grid[row][col];
      // }
    },
    dfs: async function() {
      const stack = [];
      const src = this.grid[5][12];
      const dest = this.grid[3][26];
      const rows = this.grid.length;
      const cols = this.grid[0].length;

      stack.push(src);
      src.visited = true;

      while (stack.length !== 0) {
        const u = stack.pop();
        if (u.dest === true) {
          console.log("destination found at", u.row, u.col);
          break;
        }

        for (let k = 0; k < 4; k++) {
          const dx = u.row + X[k];
          const dy = u.col + Y[k];
          // console.log(dx, dy);

          if (isvalid(dx, dy, rows, cols)) {
            const toVisit = this.grid[dx][dy];
            if (!toVisit.wall && !toVisit.visited) {
              stack.push(toVisit);
              toVisit.visited = true;
              toVisit.parent.row = u.row;
              toVisit.parent.col = u.col;
            }
          }
        }

        // console.log(parent);

        await this.sleep();
        await this.sleep();
      }
      let p = dest;
      while (true) {
        const { row, col } = p.parent;
        if (row === -1 && col === -1) {
          break;
        }

        console.log(p);
        p.visited = false;
        p.wall = false;
        p.source = false;
        p.dest = false;
        p.showPath = true;
        await this.sleep();
        await this.sleep();
        await this.sleep();
        p = this.grid[row][col];
      }
    },
    bfs: async function() {
      var queue = [];

      const src = this.grid[5][12];
      const dest = this.grid[3][26];
      const rows = this.grid.length;
      const cols = this.grid[0].length;

      queue.push(src);

      src.visited = true;
      while (queue.length !== 0) {
        const u = queue.shift();

        if (u.dest === true) {
          console.log("destination found at", u.row, u.col);
          break;
        }
        console.log("eotking");

        for (let k = 0; k < 4; k++) {
          const dx = u.row + X[k];
          const dy = u.col + Y[k];
          console.log(dx, dy);

          if (isvalid(dx, dy, rows, cols)) {
            const toVisit = this.grid[dx][dy];
            if (!toVisit.wall && !toVisit.visited) {
              queue.push(toVisit);
              toVisit.visited = true;
              toVisit.parent.row = u.row;
              toVisit.parent.col = u.col;
            }
          }
        }

        await this.sleep();
      }

      let p = dest;
      while (true) {
        const { row, col } = p.parent;
        if (row === -1 && col === -1) {
          break;
        }

        console.log(p);
        p.visited = false;
        p.wall = false;
        p.source = false;
        p.dest = false;
        p.showPath = true;
        await this.sleep();
        await this.sleep();
        await this.sleep();
        p = this.grid[row][col];
      }
    },
  },
};
</script>

<style></style>
