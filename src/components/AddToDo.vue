<template>
  <!-- report -->
  <h4>Pending {{ countPending }}</h4>
  <h4>Low {{ countLowPriority }}</h4>
  <h4>Medium {{ countMediumPriority }}</h4>
  <h4>High {{ countHighPriority }}</h4>

  <form @submit.prevent="addTodo">
    <label for="name">Todo Name:</label>
    <input
      type="text"
      id="name"
      v-model="newTodo.name"
      required
    />

    <label for="priority">Priority:</label>
    <select
      id="priority"
      v-model="newTodo.priority"
      required
    >
      <option
        value=""
        disabled
      >
        Select Priority
      </option>
      <option value="Low">Low</option>
      <option value="Medium">Medium</option>
      <option value="High">High</option>
    </select>

    <button type="submit">Add Todo</button>
  </form>
  <!-- TO DO -->
  <h1 class="text-3xl font-bold underline">Todo List</h1>
  <div
    v-for="(list, index) in todoList"
    :key="index"
  >
    <p>{{ list.name }} || prioritas : {{ list.priority }}</p>
    <div>index ke {{ index }}</div>
    <button @click="deleteToDo(index)">Delete</button>
  </div>

  <!-- DONE -->
  <h1 class="text-3xl font-bold underline">Done</h1>
  <div
    v-for="(list, index) in todoDone"
    :key="index"
  >
    {{ list.name }} || prioritas : {{ list.priority }}
  </div>
</template>

<script>
export default {
  data() {
    return {
      newTodo: { name: '', priority: '' },
      todoList: [
        { name: 'Belanja bahan masakan', priority: 'High' },
        { name: 'Rapat proyek', priority: 'Medium' },
        { name: 'Periksa email', priority: 'Low' },
      ],
      todoDone: [
        { name: 'Buat laporan', priority: 'High' },
        { name: 'Olahraga', priority: 'Low' },
      ],
    };
  },
  computed: {
    countPending() {
      return this.todoList.length;
    },
    countLowPriority() {
      let counts = 0;
      for (let i = 0; i < this.todoList.length; i++) {
        if (this.todoList[i].priority == 'Low') {
          counts += 1;
        }
      }
      return counts;
    },
    countMediumPriority() {
      let counts = 0;
      for (let i = 0; i < this.todoList.length; i++) {
        if (this.todoList[i].priority == 'Medium') {
          counts += 1;
        }
      }
      return counts;
    },
    countHighPriority() {
      let counts = 0;
      for (let i = 0; i < this.todoList.length; i++) {
        if (this.todoList[i].priority == 'High') {
          counts += 1;
        }
      }
      return counts;
    },
  },
  methods: {
    addTodo() {
      if (this.newTodo.name && this.newTodo.priority) {
        this.todoList.push({ ...this.newTodo });
        this.newTodo = { name: '', priority: '' };
      }
    },
    deleteToDo(index) {
      const done = this.todoList.splice(index, 1);
      this.todoDone.push(done[0]);
    },
  },
};
</script>
