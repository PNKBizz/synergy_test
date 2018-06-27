<template>
  <div id="app">
    <div class="form-group">
      <label for="sector">Sector</label>
      <select class="form-control" id="sector" v-model="selectedSector">
        <option v-for="sector in sectors" :key="sector.id" :value="sector">{{sector.name}}</option>
      </select>
    </div>
    <div class="form-group">
      <label for="Category">Category</label>
      <select class="form-control" id="Category" v-model="selectedCategory">
        <option v-for="category in computedСategories" :key="category.id" :value="category">{{category.name}}</option>
      </select>
    </div>
    <div class="form-group">
      <label for="Line">Line</label>
      <select class="form-control" id="Line" v-model="selectedLine">
        <option v-for="line in computedLines" :key="line.id" :value="line">{{line.name}}</option>
      </select>
    </div>
    <div class="form-group">
      <label for="Seat">Seat</label>
      <select class="form-control" id="Seat" v-model="selectedSeat">
        <option v-for="seat in lineSeats" :key="seat.id" :value="seat">{{seat.seat}}</option>
      </select>
    </div>
    <button type="button" class="btn btn-primary" @click="showSeat" :disabled="!this.selectedSeat">Save</button>
  </div>
</template>

<script>
import data from './assets/data.js'
import 'bootstrap/dist/css/bootstrap.min.css'

const { sectors, lines, categories, seats } = data.response

export default {
  name: 'app',
  data: () => ({
    sectors,
    selectedSector: null,
    lines,
    selectedLine: null,
    categories,
    selectedCategory: null,
    seats,
    selectedSeat: null
  }),
  computed: {
    sectorSeats() {
      const sectorSeats = []
      if (this.selectedSector) {
        for (let i in this.seats) {
          if (this.seats[i].status === 0 && this.seats[i].sector === this.selectedSector.id) sectorSeats.push(this.seats[i])
        }
      }
      return sectorSeats
    },
    computedСategories() {
      return this.sectorSeats
        .reduce((categories, seat) => categories.includes(seat.category) ? categories : categories.concat([seat.category]), [])
        .map(cat => this.categories[cat])
    },
    categorySeats() {
      return this.selectedCategory ? this.sectorSeats.filter(seat => seat.category === this.selectedCategory.id) : []
    },
    computedLines() {
      return this.categorySeats
        .reduce((lines, seat) => lines.includes(seat.line) ? lines : lines.concat([seat.line]), [])
        .map(line => this.lines[line])
    },
    lineSeats() {
      return this.selectedLine ? this.categorySeats.filter(seat => seat.line === this.selectedLine.id) : []
    }
  },
  methods: {
    showSeat() {
      alert(JSON.stringify(this.selectedSeat));
    }
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  padding: 50px;
}
</style>
