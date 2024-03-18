<template>
  <div class="line-graph">
    <div class="graph__content">
      <div class="graph-img">
        <div class="line-x line-wrapper">
          <div class="line"></div>
          <img src="../img/arrow.svg" class="arrow" />
          <span class="line-title">t</span>
        </div>
        <div class="line-y line-wrapper">
          <div class="line"></div>
          <img src="../img/arrow.svg" class="arrow" />
          <span class="line-title">a</span>
        </div>
        <div class="line-xy">
          <span class="line-title line-title-xy"></span>
          <div class="line"></div>
        </div>
      </div>
      <div class="graph__info">
        <h1 class="graph__header">a = kt + b</h1>
        <p class="graph__subheader">
          График вида a=kt+b, где k и b-коэфициенты, b-коэфициент, t-время,
          a-ускорение. Ускорение за равные промежутки времени изменяется на
          равные величины.
        </p>
        <p class="graph__subheader"></p>
        <div class="inputs">
          <input
            type="text"
            placeholder="Введите время (с)"
            v-model="t"
            class="input"
            @input="calc"
          />
          <input
            type="text"
            placeholder="Формула ускорения вида a=kt (ускорение в м/с^2)"
            v-model="formula"
            class="input"
            @input="calc"
          />
          <button @click="calc" class="input-btn">Посчитать</button>
          <input
            type="text"
            placeholder="Результат (м)"
            v-model="s"
            class="input input-res"
            disabled
          />
        </div>
      </div>
    </div>
    <div class="graph__logic">
      <div class="logic__header">Алгоритм считывания графика</div>
      <div class="logic__content">
        <div class="logic-point">
          1. График вида a=kt+b переобразуется в ступенчатый график, где его
          горизонтальные линии имеют длину в Δt=0.1 секунды.
        </div>
        <div class="logic-point">
          2. Горизонтальные линии являются средними значениями для участка в 0.1
          секунд (средние ускорение)
        </div>
        <div class="logic-point">
          3. Для каждой ступеньки можно расчитать расстояние, на которое
          сдвинулось тело в этот промежуток времени.
        </div>
        <div class="logic-point">4. Все расстояния суммируются.</div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref } from "vue";

const s = ref("");
const t = ref("");
const formula = ref("");

const calc = () => {
  let k = +formula.value.split("=")[1].split("t")[0];
  let b = +formula.value.split("=")[1].split("t")[1];

  //Среднее ускорение тела в первую 0.1 секунду, a=(b + 0,1k + b) / 2
  let a = (2 * b + 0.1 * k) / 2;

  //Путь, пройденный телом в первую 0.1 секунду по формуле s = a*t^2/2
  s.value = (a * 0.01) / 2;

  //Скорость, приобретенная телом в первую 0.1 секунду (начальная скорость = at)
  let v = (a / 2) * 0.1;

  for (let i = 1; i <= t.value * 10; i++) {
    s.value =
      s.value +
      v * 0.1 +
      (((i * 0.1 * k + b + ((i * 0.1 + 0.1) * k + b)) / 2) * 0.01) / 2;
    v = v + ((i * 0.1 * k + b + (i * 0.1 * k + b)) / 2) * 0.1;
  }
};
</script>

<style scoped>
@import url("https://fonts.googleapis.com/css2?family=Montserrat:wght@100..900&display=swap");
.line-graph {
  height: 100vh;
  width: 100%;
  font-family: "Montserrat", sans-serif;
}
.graph__content {
  padding-top: 64px;
  max-width: 1440px;
  margin: 0 auto;
  display: flex;
  gap: 144px;
  align-items: center;
}
.graph-img {
  position: relative;
  width: 500px;
  height: 500px;
  transition: all 0.3s;
}
.graph-img:hover .line-x {
  top: 43%;
}
.graph-img:hover .line-y {
  top: 56%;
}
.graph-img:hover .line-xy {
  left: -7%;
  transform: rotate(0deg);
  top: 30%;
}
.graph-img:hover .line-title-xy:before {
  content: "y = n";
}
.line-y {
  transform: rotate(-90deg);
}
.line-wrapper {
  width: 100%;
  position: absolute;
  display: flex;
  align-items: center;
  top: 50%;
  transition: all 1s;
}
.line {
  width: 100%;
  height: 1px;
  background-color: black;
}
.arrow {
  width: 10px;
  height: 10px;
}
.line-title {
  font-size: 16px;
  display: flex;
}
.line-xy {
  position: absolute;
  width: 100%;
  top: 50%;
  left: 0;
  display: flex;
  flex-direction: column;
  transform: rotate(-45deg) translate(0, -50%);
  transition: all 1s;
}
.line-title-xy:before {
  content: "y = kx + b";
}
.graph__header {
  font-family: "Playfair Display", serif;
  font-size: 64px;
  font-weight: 400;
  margin-bottom: 32px;
}
.graph__subheader {
  font-size: 20px;
  max-width: 600px;
}
.inputs {
  margin-top: 36px;
  display: flex;
  flex-direction: column;
  gap: 12px;
}
.input {
  width: 100%;
  padding: 12px 16px;
  border: none;
  outline: none;
  background: none;
  color: black;
  border: 1px solid black;
  border-radius: 32px;
  font-size: 16px;
  transition: all 0.3s;
}
.input:focus {
  transform: translateY(2px);
}
.input-btn {
  width: 240px;
  padding: 12px 24px;
  background: none;
  border: none;
  border-radius: 32px;
  font-size: 16px;
  border: 1px solid rgb(216, 175, 105);
  color: white;
  background-color: rgb(255, 207, 124);
  transition: all 0.3s;
}
.input-btn:hover {
  cursor: pointer;
  background-color: rgb(214, 174, 104);
}
.graph__logic {
  max-width: 1440px;
  margin: 64px auto 0 auto;
  display: flex;
  flex-direction: column;
  gap: 32px;
  font-size: 24px;
}
.logic__header {
  font-size: 32px;
  text-align: center;
}
.logic__content {
  display: flex;
  flex-direction: column;
  gap: 8px;
}

@media screen and (width < 900px) {
  .graph__content {
    justify-content: center;
    gap: 24px;
  }
  .graph-img {
    display: none;
  }
  .logic__content {
    padding: 10px;
  }
  .graph__info {
    padding: 10px;
  }
}
@media screen and (width < 660px) {
  .graph__content {
    padding-top: 32px;
  }
  .graph__subheader,
  .logic-point {
    font-size: 16px;
  }
  .graph__logic {
    margin-top: 24px;
  }
}
</style>
