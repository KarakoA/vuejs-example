<template>
  <div>
    <ul>
      <li v-for="problem in problems" :key="problem.id">
        <div style="margin-left: 5px; margin-top: 20px">
          <form @submit.prevent>
            <span>{{ problem.a }}</span>
            <span v-if="problem.isAddProblem"> + </span>
            <span v-if="!problem.isAddProblem"> - </span>
            <span> {{ problem.b }}</span> =
            <span><input class="question" v-model="problem.answer" /></span>
             <button
              class="btn btn-primary"
              style="margin-left: 5px"
              :disabled="!may_check(problem)"
              type="submit"
              @click="check_answer_add(problem)"
              v-if="problem.isAddProblem"
            >
              Check
            </button>
             <button
              class="btn btn-primary"
              style="margin-left: 5px"
              :disabled="!may_check(problem)"
              type="submit"
              v-if="!problem.isAddProblem"
              @click="check_answer_sub(problem)"
            >
              Check
            </button>
            <button
              class="btn btn-success"
              style="margin-left: 5px"
              disabled="1"
              v-if="problem.right === true"
            >
              Right
            </button>
            <button
              class="btn btn-danger"
              style="margin-left: 5px"
              disabled="1"
              v-if="problem.right === false"
            >
              Wrong
            </button>
            <button
              class="btn btn-info"
              style="margin-left: 5px"
              @click="new_problem()"
            >
              New Problem
            </button>
          </form>
        </div>
      </li>
    </ul>
    <hr />
    <div style="margin-left: 5px">
      <table>
        <tr>
          <td><strong>Statistics&nbsp;</strong></td>
          <td><span class="badge badge-success">Right</span></td>
          <td><span class="badge badge-danger">Wrong</span></td>
          <td><span class="badge badge-info">Accuracy</span></td>
        </tr>
        <tr>
          <td>
            <span> Addition </span>
          </td>
          <td>
            <span> {{ count_right_add }}</span>
          </td>
          <td>
            <span> {{ count_wrong_add }}</span>
          </td>
          <td>
            <span> {{ accuracy_add }}</span>
          </td>
        </tr>

        <tr>
          <td>
            <span> Subtraction </span>
          </td>
          <td>
            <span> {{ count_right_sub }}</span>
          </td>
          <td>
            <span> {{ count_wrong_sub }}</span>
          </td>
          <td>
            <span> {{ accuracy_sub }}</span>
          </td>
        </tr>
      </table>
    </div>
  </div>
</template>

<script>
export default {
  name: "HelloWorld",
  props: {
    msg: String,
  },
  data() {
    return {
      problems: [],
      count_right_add: 0,
      count_wrong_add: 0,
      count_right_sub: 0,
      count_wrong_sub: 0,
    };
  },
  computed: {
    accuracy_add: function () {
      let n = this.count_wrong_add + this.count_right_add;
      return n == 0 ? 0 : this.count_right_add / n;
    },
    accuracy_sub: function () {
      let n = this.count_wrong_sub + this.count_right_sub;
      return n == 0 ? 0 : this.count_right_sub / n;
    },
  },
  created: function () {
    this.add_problem();
  },
  methods: {
    add_problem() {
      let max = 100;

      let isAddProblem = Math.random() > 0.5;

      let c1 = Math.floor(Math.random() * (max - 1)) + 1;
      let a1 = Math.floor(Math.random() * (c1 - 2)) + 1;
      let b1 = c1 - a1;
      let id = this.problems.length + 1;

      let problem = isAddProblem
        ? {
            c: c1,
            a: a1,
            b: b1,
            id,
            answer: undefined,
            right: undefined,
            isAddProblem,
          }
        : {
            c: b1,
            a: c1,
            b: a1,
            id,
            answer: undefined,
            right: undefined,
            isAddProblem,
          };
      this.problems.push(problem);
    },

    check_answer_sub(problem) {
      problem.right = problem.c === parseInt(problem.answer);
        problem.right
          ? (this.count_right_sub += 1)
          : (this.count_wrong_sub += 1);
    },
    check_answer_add(problem) {
      problem.right = problem.c === parseInt(problem.answer);
        problem.right
          ? (this.count_right_add += 1)
          : (this.count_wrong_add += 1);
    },
    may_check(problem) {
      // answer non-empty and right undefined
      return (
        !(problem.answer === undefined || problem.answer === "") &&
        problem.right === undefined
      );
    },
    new_problem() {
      this.add_problem();
    },
  },
};
</script>
<style scoped>
.index {
  padding: 0 20px 0 5px;
}

.question {
  font-color: white;
  width: 2em;
  background-color: #3ff;
  margin-left: 5px;
  margin-right: 5px;
}
</style>
