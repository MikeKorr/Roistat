<template>
  <div class="person-node" @click.stop="toggleSubordinateVisibility">
    <div class="person__row">
      <span class="person__info">{{ node.name }}</span>
      <span class="person__info">{{ node.phone }}</span>
    </div>

    <ul v-if="node.subordinate?.length" v-show="isVisibleSubordinate">
      <li v-for="person in node.subordinate" :key="person.id">
        <node :node="person" />
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  name: "node",
  props: {
    node: Object,
  },
  data() {
    return {
      isVisibleSubordinate: false,
    };
  },
  methods: {
    toggleSubordinateVisibility() {
      this.isVisibleSubordinate = !this.isVisibleSubordinate;
    },
  },
};
</script>

<style lang="scss" scoped>
.person-node {
  cursor: pointer;
  gap: 10px 20px;
  border: 1px solid #000;
  width: 100%;
  border: none;

  .person__row {
    display: flex;
    justify-content: space-between;
  }

  li {
    list-style-type: none;
  }

  ul {
    margin-left: 20px;
  }

  .person__info {
    padding: 8px;
    font-size: 15px;
    font-weight: 700;
  }
}
</style>
