<template>
  <div class="backlog-view">
    <new-item></new-item>

    <div class="card" v-for="item in items" :key="item.id">
      <div class="card-block">
        <h5 class="card-title"><span class="text-muted">#{{item.id}}</span>
          {{item.text}}
          <button type="button" class="close-button pull-right" @click="removeItem(item)">
            <span>&times;</span>
          </button>
          <span :class="badgeClass(item)">{{badgeText(item)}}</span>
        </h5>
      </div>
    </div>
  </div>
</template>

<script>
import { mapState } from 'vuex';
import NewItem from './NewItem';

const badgeDetail = {
  todo: {
    text: 'todo',
    class: 'badge badge-dark',
  },
  inProgress: {
    text: 'in progress',
    class: 'badge badge-info',
  },
  done: {
    text: 'done',
    class: 'badge badge-success',
  },
};

export default {
  name: 'Backlog',
  components: {
    'new-item': NewItem,
  },
  computed: mapState({
    items: s => [...s.items.todo, ...s.items.inProgress, ...s.items.done],
  }),
  methods: {
    itemLane(item) {
      if (this.$store.state.items.todo.includes(item)) {
        return 'todo';
      } else if (this.$store.state.items.inProgress.includes(item)) {
        return 'inProgress';
      }
      return 'done';
    },
    badgeText(item) {
      const lane = this.itemLane(item);
      return badgeDetail[lane].text;
    },
    badgeClass(item) {
      const lane = this.itemLane(item);
      return `${badgeDetail[lane].class} pull-right`;
    },
    removeItem(item) {
      this.$store.commit('removeItem', item);
    },
  },
};
</script>
