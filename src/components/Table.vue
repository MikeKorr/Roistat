<template>
  <div class="table">
    <header>
      <span class="table__titles" @click="sortData('name')">Имя</span>
      <span class="table__titles">Телефон</span>
    </header>

    <PersonNode v-for="person in sortedData" :key="person.id" :node="person" />
  </div>
</template>

<script>
import PersonNode from "@/components/PersonNode.vue";

const SORT_TYPE = {
  ASC: "ASC",
  DESC: "DESC",
};

export default {
  name: "Table",
  components: { PersonNode },
  props: {
    people: {
      type: Array,
      required: true,
    },
  },
  data() {
    return {
      sortedData: this.people,
      sort: SORT_TYPE.ASC,
    };
  },
  methods: {
    sortData(column) {
      const copyPeople = [...this.people];
      copyPeople.sort((a, b) => {
        if (a[column] < b[column]) return this.sort === SORT_TYPE.ASC ? -1 : 1;
        if (a[column] > b[column]) return this.sort === SORT_TYPE.ASC ? 1 : -1;
        return 0;
      });
      this.sortedData = copyPeople;
      this.sort = this.sort === SORT_TYPE.ASC ? SORT_TYPE.DESC : SORT_TYPE.ASC;
    },
  },
};
</script>

<style lang="scss" scoped>
.table {
  border: 2px solid #000;
  border-radius: 6px;
  height: 300px;
  width: 500px;
  display: flex;
  align-items: start;
  justify-content: start;
  flex-direction: column;

  .table__titles {
    font-size: 20px;
    color: green;
    font-weight: 700;
  }

  header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    width: 100%;
    border-bottom: 2px solid green;
  }
}
</style>
