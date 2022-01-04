<template>
  <div class="carousel">
    <!-- Navigation -->
    <div v-if="navigationEnabled" class="navigate">
      <div class="toggle-page left">
        <svg
          xmlns="http://www.w3.org/2000/svg"
          class="icon"
          fill="none"
          viewBox="0 0 24 24"
          stroke="currentColor"
          @click="prevSlide"
        >
          <path
            stroke-linecap="round"
            stroke-linejoin="round"
            stroke-width="2"
            d="M15 19l-7-7 7-7"
          />
        </svg>
      </div>
      <div class="toggle-page right">
        <svg
          xmlns="http://www.w3.org/2000/svg"
          class="icon"
          fill="none"
          viewBox="0 0 24 24"
          stroke="currentColor"
          @click="nextSlide"
        >
          <path
            stroke-linecap="round"
            stroke-linejoin="round"
            stroke-width="2"
            d="M9 5l7 7-7 7"
          />
        </svg>
      </div>
    </div>

    <!-- Pagination -->
    <div v-if="paginationEnabled" class="pagination">
      <span
        @click="goToSlide(index)"
        v-for="(slide, index) in getSlideCount"
        :key="index"
        :class="{ active: index + 1 === currentSlide }"
      >
      </span>
    </div>
  </div>
</template>

<script>
import { ref, onMounted } from "vue";
export default {
  props: ["startAutoPlay", "timeout", "navigation", "pagination"],
  setup(props) {
    const currentSlide = ref(1);
    const getSlideCount = ref(null);
    const autoPlayEnabled = ref(
      props.startAutoPlay === undefined ? true : props.startAutoPlay
    );
    const timeoutDuration = ref(
      props.timeout === undefined ? 5000 : props.timeout
    );
    const paginationEnabled = ref(
      props.pagination === undefined ? true : props.pagination
    );
    const navigationEnabled = ref(
      props.navigation === undefined ? true : props.navigation
    );

    // next slide
    const nextSlide = () => {
      if (currentSlide.value === getSlideCount.value) {
        currentSlide.value = 1;
        return;
      }
      currentSlide.value += 1;
    };

    // prev slide
    const prevSlide = () => {
      if (currentSlide.value === 1) {
        currentSlide.value = 1;
        return;
      }
      currentSlide.value -= 1;
    };
    // Clique em paginacao
    const goToSlide = (index) => {
      currentSlide.value = index + 1;
    };
    // autoplay
    const autoplay = () => {
      setInterval(() => {
        nextSlide();
      }, timeoutDuration.value);
    };

    // roda autoplay automaticamente caso enabled = true
    if (autoPlayEnabled.value) {
      autoplay();
    }
    // conta o numero de slides dinamicamente
    onMounted(() => {
      getSlideCount.value = document.querySelectorAll(".slide").length;
    });
    return {
      currentSlide,
      nextSlide,
      prevSlide,
      getSlideCount,
      goToSlide,
      paginationEnabled,
      navigationEnabled,
    };
  },
};
</script>

<style lang="scss">
.navigate {
  padding: 0 16px;
  height: 100%;
  width: 100%;
  position: absolute;
  display: flex;
  justify-content: center;
  align-items: center;

  .toggle-page {
    height: 18px;
    width: 18px;
    display: flex;
    flex: 1;
  }
  .right {
    justify-content: flex-end;
  }
  .icon {
    cursor: pointer;
    display: flex;
    align-items: center;
    justify-content: center;
    border-radius: 50%;
    width: 40px;
    height: 40px;
    background-color: #6347c7;
    color: #fff;
  }
}
.pagination {
  position: absolute;
  bottom: 24px;
  width: 100%;
  display: flex;
  gap: 16px;
  justify-content: center;
  align-items: center;

  span {
    cursor: pointer;
    width: 20px;
    height: 20px;
    border-radius: 50%;
    background-color: #fff;
    box-shadow: 0 1px 3px 0 rgba($color: #000000, $alpha: 0.1),
      0 1px 2px 0 rgba($color: #000000, $alpha: 0.06);
  }
  .active {
    background-color: #6347c7;
  }
}
</style>
