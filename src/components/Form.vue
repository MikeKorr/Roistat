<template>
  <form class="form" @submit.prevent="handleSubmit">
    <div class="input-wrapper">
      <label for="name">Имя</label>
      <input id="name" type="text" v-model="person.name" placeholder="Имя" />
    </div>

    <div class="input-wrapper">
      <label for="phone">Телефон</label>
      <input
        id="phone"
        type="tel"
        v-model="person.phone"
        placeholder="Номер телефона"
      />
    </div>

    <div class="input-wrapper">
      <label for="boss">Начальник</label>

      <select name="boss" id="boss" v-model="person.bossId">
        <option v-for="man in allPeople" :key="man.id" :value="man.id">
          {{ man.name }}
        </option>
      </select>
    </div>

    <button type="submit" :disabled="!person.name || !person.phone">
      Сохранить
    </button>
  </form>
</template>

<script>
import { v4 as uuidv4 } from "uuid";
export default {
  name: "Form",
  props: {
    people: {
      type: Array,
      required: true,
    },
  },
  data() {
    return {
      person: {
        id: "",
        name: "",
        phone: "",
        bossId: "",
        subordinate: [],
      },
    };
  },

  computed: {
    allPeople() {
      const result = [];

      function extractSubordinate(people = []) {
        for (let person of people) {
          result.push(person);
          if (person.subordinate?.length) {
            extractSubordinate(person.subordinate);
          }
        }
      }

      extractSubordinate(this.people);
      return result;
    },
  },
  methods: {
    handleSubmit() {
      this.person.id = uuidv4();
      this.$emit("handleSubmit", this.person);
      this.$emit("update:visible", false);
    },
  },
};
</script>

<style lang="scss" scoped>
.form {
  .input-wrapper {
    display: flex;
    justify-content: space-between;
    gap: 0 20px;
    margin-bottom: 10px;
    input,
    select {
      box-sizing: border-box;
      width: 160px;
    }
  }
}
</style>
