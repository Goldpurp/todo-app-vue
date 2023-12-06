<script setup>
import { ref } from "vue";
import { Icon } from "@iconify/vue";
import { v4 as uuid } from "uuid";
import TaskCard from "./components/TaskCard.vue";

const tasks = ref([
  {
    id: uuid(),
    editing: false,
    completed: false,
    task: "test data",
    created: Date.now(),
  },
]);
const currentInput = ref("");

function handleDelete(id) {
  tasks.value = tasks.value.filter((task) => task.id != id);
}

function handleEdit(index) {
  tasks.value[index].editing = !tasks.value[index].editing;
}

function handleCompleted(index) {
  tasks.value[index].completed = !tasks.value[index].completed;
}

function handleSave(index, value) {
  tasks.value[index].task = value;
  tasks.value[index].editing = !tasks.value[index].editing;
}

function handleSubmit() {
  const newTask = {
    id: uuid(),
    editing: false,
    completed: false,
    created: Date.now(),
    task: currentInput.value,
  };
  tasks.value.push(newTask);
  currentInput.value = "";
}
</script>

<template>
  <div class="wrapper">
    <div class="hero">
      <h1>Render</h1>
      <h2>Your personal buddy</h2>
    </div>
    <form @submit.prevent="handleSubmit">
      <section class="hero-footer">
        <div>
          <input
            id="Input"
            type="text"
            class="input"
            autocomplete="off"
            placeholder="Enter a task"
            v-model="currentInput"
          />
        </div>
        <button :disabled="!currentInput" type="submit" class="Add">
          <Icon icon="line-md:plus" />
        </button>
      </section>
    </form>
    <div class="card-list">
      <TaskCard
        v-for="(task, index) in tasks"
        :id="task.id"
        :index="index"
        :task="task.task"
        @edit="handleEdit"
        @save="handleSave"
        @delete="handleDelete"
        :created="task.created"
        :editing="task.editing"
        :completed="task.completed"
        @completed="handleCompleted"
      />
    </div>
  </div>
</template>

<style scoped>
.wrapper {
  width: 100%;
  padding: 0 20px;
}
.hero {
  display: flex;
  margin-top: 150px;
  text-align: center;
  margin-bottom: 20px;
  flex-direction: column;
}

.hero h1 {
  font-size: 70px;
  font-weight: 800;
}

.hero h2 {
  font-size: 15px;
  font-weight: 600;
}

.input-text {
  display: block;
  color: #1e304a;
  font-size: 15px;
  font-weight: 600;
  padding: 0 20px;
}

.input {
  width: 100%;
  height: 30px;
  outline: none;
  box-shadow: none;
  padding: 0px 16px;
  border-radius: 5px;
  outline-width: 1px;
  border-radius: 15px;

  height: 40px;
  border: 1px solid #1a2f20;
}

.card-list {
}

.Add {
  width: 30px;
  height: 30px;
  border: none;
  display: flex;
  color: #273b52;
  font-size: 20px;
  border-radius: 50%;
  align-items: center;
  justify-content: center;
  border: 1px solid #1a2f20;
  background-color: #f1f5f8;
}

.hero-footer {
  gap: 20px;
  width: 100%;
  display: flex;
  align-items: center;
  margin-bottom: 20px;
  padding: 5px 20px;
  justify-content: center;
}
</style>
