<template>
  <div>
    <h1>{{ result }}</h1>
    <form @submit.prevent="onSumbmitForm">
      <input ref="answer" minlength="4" maxlength="4" v-model="value" />
      <button type="submit">입력</button>
    </form>
    <div>시도 : {{ tries.length }}</div>
    <ul>
      <li v-for="t in tries" v-bind:key="t.id">
        <div>{{ t.try }}</div>
        <div>{{ t.result }}</div>
      </li>
    </ul>
  </div>
</template>
<script>
const getNumbers = () => {
  const candidates = [1, 2, 3, 4, 5, 6, 7, 8, 9];
  const array = [];
  for (let i = 0; i < 4; i += 1) {
    const chosen = candidates.splice(Math.floor(Math.random() * (9 - i)), 1)[0];
    array.push(chosen);
  }
  return array;
};
export default {
  data() {
    return {
      answer: getNumbers(),
      tries: [], // 시도
      value: "", // 입력
      result: "", // 결과
    };
  },
  methods: {
    onSumbmitForm() {
      if (this.value === this.answer.join("")) {
        // 성공시
        this.tries.push({
          try: this.value,
          result: "홈런",
        });
        this.result = "홈런";
        alert("새로운 게임을 시작합니다");
        this.value = "";
        this.answer = getNumbers();
        this.tries = [];
        this.$refs.answer.focus();
      } else {
        if (this.tries.length >= 9) {
          this.result = `10번동안 맞추지 못했습니다! 답은 ${this.answer.join(
            ","
          )}입니다.`;
          alert("새로운 게임을 시작합니다");
          this.value = "";
          this.answer = getNumbers();
          this.tries = [];
          this.$refs.answer.focus();
        } else {
          let strike = 0;
          let ball = 0;
          const answerArray = this.value.split("").map((v) => parseInt(v));
          for (let i = 0; i < 4; i++) {
            if (answerArray[i] === this.answer[i]) {
              strike++;
            } else if (this.answer.includes(answerArray[i])) {
              ball++;
            }
          }
          this.tries.push({
            try: this.value,
            result: `${strike} 스트라이크 ${ball} 볼`,
          });
          this.value = "";
          this.$refs.answer.focus();
        }
      }
    },
  },
};
</script>

<style></style>
