<script>
import { onMounted, ref } from 'vue';

(function () {
  let orderBox = document.getElementById('order-box');
  let pointerElem = document.getElementById('boxElem');

  function onMouseMove(event) {
    let mouseX = event.pageX;
    let mouseY = event.pageY;
    let crd = orderBox.getBoundingClientRect();
    let activePointer = crd.left <= mouseX && mouseX <= crd.right && crd.top <= mouseY && mouseY <= crd.bottom;

    requestAnimationFrame(function movePointer() {
      if (activePointer) {
        pointerElem.classList.remove('box-pointer-hidden');
        pointerElem.style.left = Math.floor(mouseX) + 'px';
        pointerElem.style.top = Math.floor(mouseY) + 'px';
      } else {
        pointerElem.classList.add('box-pointer-hidden');
      }
    });
  }

  function disablePointer() {
    pointerElem.style.left = "83px";
    pointerElem.style.top = "83px";
    requestAnimationFrame(function hidePointer() {
      pointerElem.classList.add('box-pointer-hidden');
    });
  }

  if (orderBox) {
    orderBox.addEventListener('mousemove', onMouseMove, false);
    orderBox.addEventListener('mouseleave', disablePointer, false);
  }


})();

export default {
  setup() {
    const orderRef = ref(null);
    onMounted(() => {
      let prev = window.pageYOffset;
      window.addEventListener("scroll", () => {
        let curr = window.pageYOffset;
        if (prev < curr) {
          orderRef.value.classList.add("scrollBottom");
        } else {
          orderRef.value.classList.remove("scrollBottom");
        }
        if (curr === 0) {
          orderRef.value.classList.remove("scrollBottom");
        }
      });
    });
    return { orderRef };
  }
}
</script>

<template>
  <div id="order-box">
    <div ref="orderRef" class="order" id="boxElem">
      <p>Заказать
        уборку
      </p>
    </div>
  </div>

</template>

<style>
#order-box {
  width: 400px;
  height: 200px;
  display: flex;
  align-content: center;
  justify-content: center;
  position: fixed;
  left: 0;
  right: 0;
  bottom: 0;
  margin: 0 auto;
  z-index: 111;
}

.order {
  cursor: pointer;
  width: 195px;
  height: 195px;
  position: fixed;
  left: 0;
  right: 0;
  bottom: 30px;
  margin: 0 auto;
  background: #5A30F0;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  transition: .4s;
  animation: orderBlock 1.5s linear;
}

@keyframes orderBlock {
  0% {
    transform: translateY(100%);
    opacity: 0;
  }

  100% {
    transform: translateY(0%);
    opacity: 1;
  }
}

.scrollBottom {
  bottom: -65px;
}

.order::after {
  content: '';
  position: absolute;
  left: 0px;
  top: 0px;
  width: 100%;
  height: 100%;
  border-radius: 50%;
}

.order:hover::after {
  border: 1px solid #fff;
  border-radius: 50%;
  transform: scale(1.5);
  opacity: .4;
}

.order:hover {
  content: '';
  transform: scale(1.2);
}

.order p {
  text-align: center;
  max-width: 160px;
  width: 100%;
  font-style: normal;
  font-weight: 500;
  font-size: 18px;
  line-height: 130%;
  letter-spacing: 0.03em;
  text-transform: uppercase;
  color: #FFFFFF;
  animation: orderText 10s linear infinite;
}

@keyframes orderText {
  0% {
    transform: rotate(-180deg);
  }

  100% {
    transform: rotate(180deg);
  }
}
</style>
