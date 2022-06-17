<script>
import { onMounted, ref } from 'vue';

export default {
  setup() {
    const workRef = ref(null);
    onMounted(() => {
      let prev = window.pageYOffset;
      window.addEventListener("scroll", () => {
        let curr = window.pageYOffset;
        if (prev < curr) {
          workRef.value.classList.add("workAnimations");
        }
        else {
          workRef.value.classList.remove("workAnimations");
        }
        if (curr === 0) {
          workRef.value.classList.remove("workAnimations");
        }
      });
    });
    return { workRef };
  }
}</script>

<template>
  <div ref="workRef" class="work">
    <div class="container">
      <div class="work__title">
        <h2>Как мы убираем</h2>
        <p>Клинер приезжает в назначенное время со всем необходимым и сразу приступает к делу. Вам остаётся только
          оценить результат.</p>
      </div>
      <div class="work__content">
        <img src="../image/Placeholder.jpg" alt="">
      </div>
    </div>
  </div>
</template>

<style>
@import './work.css';
</style>