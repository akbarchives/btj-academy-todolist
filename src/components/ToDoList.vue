<template>
  <div class="container mx-auto w-full p-8 lg:w-1/2">
    <h1 class="my-4 text-3xl font-bold">To Do App</h1>
    <!-- Report -->
    <div class="flex flex-wrap gap-4 py-5 text-center">
      <div
        class="h-20 w-20 rounded-md border bg-slate-200 p-2 transition hover:bg-slate-300"
      >
        <p class="font-medium">Pending</p>
        <p class="text-2xl font-bold">{{ countPending }}</p>
      </div>
      <div
        class="h-20 w-20 rounded-md border bg-blue-200 p-2 transition hover:bg-blue-300"
      >
        <p class="font-medium">Low</p>
        <p class="text-2xl font-bold">{{ countLowPriority }}</p>
      </div>
      <div
        class="h-20 w-20 rounded-md border bg-orange-200 p-2 transition hover:bg-orange-300"
      >
        <p class="font-medium">Medium</p>
        <p class="text-2xl font-bold">{{ countMediumPriority }}</p>
      </div>
      <div
        class="h-20 w-20 rounded-md border bg-red-200 p-2 transition hover:bg-red-300"
      >
        <p class="font-medium">High</p>
        <p class="text-2xl font-bold">{{ countHighPriority }}</p>
      </div>
    </div>

    <!-- Add To Do -->
    <form
      @submit.prevent="addToDoList"
      class="flex flex-wrap items-end gap-2 rounded-lg border p-4 font-medium"
    >
      <div class="flex w-full flex-col sm:w-1/2">
        <label for="name">Todo Name:</label>

        <input
          class="rounded-md border px-4 py-2"
          type="text"
          id="name"
          v-model="newTodo.name"
          required
          placeholder="add task name"
        />
      </div>
      <div class="flex grow flex-col">
        <label for="priority">Priority:</label>
        <select
          id="priority"
          v-model="newTodo.priority"
          required
          class="rounded-md border px-4 py-2"
        >
          <option value="" disabled>Select Priority</option>
          <option value="High">High</option>
          <option value="Medium">Medium</option>
          <option value="Low">Low</option>
        </select>
      </div>

      <button
        type="submit"
        class="h-1/2 w-full rounded-md bg-green-500 px-4 py-2 text-white hover:bg-green-600"
      >
        Add Todo
      </button>
    </form>
    <!-- To Do List -->
    <div class="container flex flex-col gap-4 sm:flex-row">
      <div class="w-full sm:w-1/2">
        <h1 class="my-4 text-3xl font-medium">Todo</h1>
        <div
          v-for="(list, index) in toDoList"
          :key="index"
          class="my-2 flex flex-col-reverse gap-2 rounded-lg border p-2"
        >
          <div class="group flex flex-col gap-2">
            <p class="font-medium">{{ list.name }}</p>
            <div class="flex justify-between text-sm">
              <p class="flex items-center gap-1">
                <span
                  class="inline-block h-3 w-3 rounded-sm"
                  :class="getPriorityClass(list.priority)"
                  >&nbsp;</span
                >
                {{ list.priority }}
              </p>
              <div class="flex gap-2">
                <button
                  @click="deleteToDoList(index)"
                  class="invisible rounded-md bg-red-500 px-2 py-1 font-thin text-white duration-200 ease-in-out hover:bg-red-600 group-hover:visible"
                >
                  Delete
                </button>
                <button
                  @click="doneToDo(index)"
                  class="invisible rounded-md bg-green-500 px-2 py-1 font-thin text-white duration-200 ease-in-out hover:bg-green-600 group-hover:visible"
                >
                  Done
                </button>
              </div>
            </div>
          </div>
        </div>
      </div>
      <div class="w-full sm:w-1/2">
        <h1 class="my-4 text-3xl font-medium">Done</h1>
        <div
          v-for="(list, index) in toDoDone"
          :key="index"
          class="my-2 flex flex-col gap-2 rounded-lg border p-2"
        >
          <div class="group flex flex-col gap-2">
            <p class="font-medium">{{ list.name }}</p>
            <div class="flex justify-between text-sm">
              <p class="flex items-center gap-1">
                <span
                  class="inline-block h-3 w-3 rounded-sm"
                  :class="getPriorityClass(list.priority)"
                  >&nbsp;</span
                >
                {{ list.priority }}
              </p>
              <div class="flex">
                <button
                  @click="deleteDone(index)"
                  class="invisible rounded-md bg-red-500 px-2 py-1 font-thin text-white duration-200 ease-in-out hover:bg-red-600 group-hover:visible"
                >
                  Delete
                </button>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      newTodo: { name: "", priority: "" },
      toDoList: JSON.parse(localStorage.getItem("toDoList")) || [
        { name: "Belanja bahan masakan", priority: "High" },
        { name: "Rapat proyek", priority: "Medium" },
        { name: "Periksa email", priority: "Low" },
      ],
      toDoDone: JSON.parse(localStorage.getItem("toDoDone")) || [
        { name: "Buat laporan", priority: "High" },
        { name: "Olahraga", priority: "Low" },
      ],
    };
  },
  watch: {
    toDoList: {
      handler(newToDoList) {
        localStorage.setItem("toDoList", JSON.stringify(newToDoList));
      },
      deep: true,
    },
    toDoDone: {
      handler(newToDoDone) {
        localStorage.setItem("toDoDone", JSON.stringify(newToDoDone));
      },
      deep: true,
    },
  },
  computed: {
    countPending() {
      return this.toDoList.length;
    },
    countLowPriority() {
      return this.countPriority("Low");
    },
    countMediumPriority() {
      return this.countPriority("Medium");
    },
    countHighPriority() {
      return this.countPriority("High");
    },
  },
  methods: {
    countPriority(priority) {
      let counts = 0;
      for (let i = 0; i < this.toDoList.length; i++) {
        if (this.toDoList[i].priority == priority) {
          counts += 1;
        }
      }
      return counts;
    },
    addToDoList() {
      if (this.newTodo.name && this.newTodo.priority) {
        this.toDoList.push({ ...this.newTodo });
        this.newTodo = { name: "", priority: "" }; // Clear input after adding
      }
    },
    deleteToDoList(index) {
      this.toDoList.splice(index, 1);
    },
    deleteDone(index) {
      this.toDoDone.splice(index, 1);
    },
    doneToDo(index) {
      const done = this.toDoList.splice(index, 1);
      this.toDoDone.push(done[0]);
    },
    getPriorityClass(priority) {
      let low = false;
      let medium = false;
      let high = false;

      if (priority === "Low") {
        low = true;
      } else if (priority === "Medium") {
        medium = true;
      } else {
        high = true;
      }

      return {
        "bg-blue-400": low,
        "bg-orange-400": medium,
        "bg-red-400": high,
      };
    },
  },
};
</script>
