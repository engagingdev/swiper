<template>
  <swiper :scrollbar="scrollbarConfig"
          :slidesPerView="1"
          :spaceBetween="30"
          :pagination="pagination"
          :navigation="true"
          :modules="modules"
          class="mySwiper">
    <div class="swiper-pagination" slot="pagination"></div>
    <swiper-slide v-for="(square, index) in squares" :key="index" class="square">
      <div class="square-content">
        <span>{{ square.month }}</span>&nbsp;
        <span>{{ square.day }}</span>
      </div>
    </swiper-slide>
  </swiper>
</template>

<script>
import { Swiper, SwiperSlide } from 'swiper/vue';
import 'swiper/css';
import 'swiper/css/navigation';
import 'swiper/css/scrollbar';
import 'swiper/css/pagination';
import './style.css';
import { Navigation, Scrollbar, Pagination } from 'swiper/modules';

export default {
  components: {
    Swiper,
    SwiperSlide,
  },
  data() {
    return {
      squares: [], // Initialize as empty array
      pagination: {
        clickable: true,
        dynamicBullets: true,
        dynamicMainBullets: 9,
        renderBullet: (index, className) => {
          return '<span class="' + className + '">' + this.squares[index].month + '<br/>'+this.squares[index].day + '</span>'; // Arrow function to bind 'this'
        },
      },
      scrollbar: {
        dragSize: 1000,
      },
      modules: [Navigation, Scrollbar, Pagination],
    };
  },
  computed: {
    scrollbarConfig() {
      return {
        hide: false,
        enabled: true,
        draggable: true,
      };
    },
  },
  created() {
    // Populate squares array
    this.squares = this.generateDaysOfYear();
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
  },
};
</script>
<style scoped>
.swiper {
  width: 100%;
  max-width: 800px;
  height: 100%;
  max-height: 880px;
}
.swiper-slide {
  background-color: #8E8E8E;
  color: white;
  margin-top: 139px;
  height: 76%;
}

.square-content span {
  font-size: 30px;
}

</style>