<template>
  <div class="carousel-container">
    <button @click="prevSquare" :disabled="currentSquare === 0" class="control-btn prev-btn">&#8592;</button>
    <div class="carousel" :style="{ transform: `translateX(-${currentSquare * (100 / squaresPerView + 1)}%)` }">
      <div v-for="(square, index) in squares" :key="index" class="square" @click="selectSquare(square)">
        <div class="square-content">
          <div>{{ square.month }}</div>
          <div>{{ square.day }}</div>
        </div>
      </div>
    </div>
    <button @click="nextSquare" :disabled="currentSquare >= totalSquares - squaresPerView" class="control-btn next-btn">&#8594;</button>
    <div class="selected-content">
      <p>{{ selectedSquare?.month ?? "Jan" }} {{ selectedSquare?.day ?? "1" }}</p>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      squares: this.generateDaysOfYear(),
      currentSquare: 0,
      squaresPerView: 10,
      squareWidth: 9.5, // Adjusted to account for margin
      squareMargin: 0.5, // Margin percentage of the container width
      selectedSquare: null,
    };
  },
  computed: {
    totalSquares() {
      return this.squares.length;
    },
  },
  methods: {
    generateDaysOfYear() {
      const days = [];
      const months = ["Jan", "Feb", "Mar", "Apr", "May", "Jun", "Jul", "Aug", "Sep", "Oct", "Nov", "Dec"];
      const daysInMonth = [31, 28, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31];
      months.forEach((month, monthIndex) => {
        for (let day = 1; day <= daysInMonth[monthIndex]; day++) {
          days.push({ month, day });
        }
      });
      return days;
    },
    nextSquare() {
      if (this.currentSquare < this.totalSquares - this.squaresPerView) {
        this.currentSquare += 1;
      }
    },
    prevSquare() {
      if (this.currentSquare > 0) {
        this.currentSquare -= 1;
      }
    },
    selectSquare(square) {
      this.selectedSquare = square;
    },
  },
};
</script>

<style scoped>
.carousel-container {
  display: flex;
  flex-direction: column;
  align-items: center;
  width: 100%;
  overflow: hidden;
}
.carousel {
  display: flex;
  transition: transform 0.5s ease-in-out;
  width: 100%;
}
.square {
  background-color: #808080; /* grey background */
  flex: 0 0 calc(10% - 10px); /* Adjusted to account for margin */
  margin-left: 10px;
  position: relative;
  display: flex;
  align-items: center;
  justify-content: center;
  color: white;
  font-size: 1rem;
  font-weight: bold;
  text-align: center;
}
.square-content {
  display: flex;
  flex-direction: column;
}
.square:first-child {
  margin-left: 0; /* Remove margin for the first square */
}
.controls {
  margin-top: 1rem;
  display: flex;
  gap: 1rem;
}
.control-btn {
  padding: 0.5rem 1rem;
  background-color: #4a5568; /* Tailwind gray-700 */
  color: #fff;
  border: none;
  border-radius: 0.25rem;
  cursor: pointer;
}
.selected-content {
  width: 100%;
  justify-content: center;
  color: white;
  font-size: 1rem;
  font-weight: bold;
  text-align: center;
  background: gray;
  /* height: 111%; */
  padding: 18%;
}
.control-btn:disabled {
  background-color: #a0aec0; /* Tailwind gray-400 */
  cursor: not-allowed
}
</style>
