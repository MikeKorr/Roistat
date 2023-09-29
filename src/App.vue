<template>
  <div class="table">
    <teleport to="body">
      <Modal title="Добавление пользователя" v-model:visible="isVisibleModal">
        <Form
          :people="people"
          @handleSubmit="handleSubmit"
          @update:visible="isVisibleModal = $event"
        />
      </Modal>
    </teleport>
    <div>
      <Table :people="people" />

      <button
        class="table__button"
        type="button"
        @click="isVisibleModal = true"
      >
        Добавить
      </button>
    </div>
  </div>
</template>

<script>
import Modal from "@/components/Modal.vue";
import Form from "@/components/Form.vue";
import Table from "@/components/Table.vue";
export default {
  components: {
    Modal,
    Form,
    Table,
  },

  data() {
    return {
      isVisibleModal: false,
      people: [],
      sortedInfo: [],
    };
  },

  mounted() {
    const payload = JSON.parse(localStorage.getItem("people")) || [];
    Object.assign(this.people, payload);
    // this.sortedInfo = this.sortData("name");
  },
  methods: {
    findBoss(people = [], bossId) {
      for (let person of people) {
        if (person.id === bossId) {
          return person;
        }
        if (person.subordinate.length) {
          const boss = this.findBoss(person.subordinate, bossId);
          if (boss) {
            return boss;
          }
        }
      }
    },
    handleSubmit(person) {
      if (person.bossId) {
        const boss = this.findBoss(this.people, person.bossId);

        boss?.subordinate.push(person);
      } else {
        this.people.push(person);
      }

      localStorage.setItem("people", JSON.stringify(this.people));
    },
  },
};
</script>
<style lang="scss">
.table {
  display: flex;
  align-items: center;
  justify-content: center;
  height: 100vh;

  .table__button {
    background-color: #4caf50;
    border: none;
    color: white;
    padding: 15px 32px;
    text-align: center;
    text-decoration: none;
    width: 504px;
    font-size: 16px;
    border-radius: 6px;
  }
}
</style>
