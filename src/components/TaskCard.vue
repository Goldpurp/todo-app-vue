<script setup lang="ts">
import { ref, type PropType } from "vue";
import { Icon } from "@iconify/vue";

const props = defineProps({
  created: {
    type: Number as PropType<number>,
    required: true,
  },
  id: String as PropType<string>,
  task: String as PropType<string>,
  index: Number as PropType<number>,
  editing: Boolean as PropType<boolean>,
  completed: Boolean as PropType<boolean>,
});

const emits = defineEmits(["edit", "save", "completed", "delete"]);

const editedInput = ref(props.task);

function date(timestamp: number) {
  const date = new Date(timestamp);

  const formattedDate = `${("0" + date.getHours()).slice(-2)}:${(
    "0" + date.getMinutes()
  ).slice(-2)} ${getDayOfWeek(date)} (${getMonthAbbreviation(
    date
  )} ${date.getDate()})`;

  function getMonthAbbreviation(date) {
    const months = [
      "Jan",
      "Feb",
      "Mar",
      "Apr",
      "May",
      "Jun",
      "Jul",
      "Aug",
      "Sep",
      "Oct",
      "Nov",
      "Dec",
    ];
    return months[date.getMonth()];
  }

  function getDayOfWeek(date: Date) {
    const daysOfWeek = ["Sun", "Mon", "Tue", "Wed", "Thu", "Fri", "Sat"];
    return daysOfWeek[date.getDay()];
  }

  return formattedDate;
}
</script>

<template>
  <div class="card">
    <div class="checkbox">
      <button
        v-if="!props.completed"
        class="done"
        @click="$emit('completed', index)"
      >
        <Icon icon="line-md:circle" />
      </button>

      <button v-else class="done" @click="$emit('completed', index)">
        <Icon icon="line-md:confirm-circle" />
      </button>
      <div>
        <p
          v-if="!props.editing"
          :class="{
            completed: props.completed,
          }"
        >
          {{ props.task }}
        </p>
        <input type="text" v-else v-model="editedInput" />
        <h2>{{ date(props.created) }}</h2>
      </div>
    </div>

    <div class="Btn-holder">
      <button
        class="Edit"
        v-if="!props.editing"
        @click="$emit('edit', props.index)"
      >
        <Icon icon="line-md:edit-twotone" />
      </button>
      <button
        v-else
        class="Edit"
        @click="$emit('save', props.index, editedInput)"
      >
        <Icon icon="line-md:confirm" />
      </button>
      <button
        v-if="!props.editing"
        class="Delete"
        @click="$emit('delete', props.id)"
      >
        <Icon icon="line-md:buy-me-a-coffee-filled" />
      </button>
      <button v-else class="Delete" @click="emits('edit', props.index)">
        <Icon icon="line-md:cancel" />
      </button>
    </div>
  </div>
</template>

<style scoped>
.card h2 {
  font-size: 10px;
  font-weight: 200;
}

h1 {
  font-size: 70px;
  font-weight: 900;
}

p {
  font-size: 15px;
}

input {
  width: 100%;
  height: 30px;
  outline: none;
  box-shadow: none;
  padding: 0px 16px;
  border-radius: 5px;
  outline-width: 1px;
  border: 1px solid rgb(219, 219, 219);
}
.card {
  width: 100%;
  display: flex;
  padding: 20px;
  text-align: left;
  color: #ffffff;
  margin-bottom: 15px;
  align-items: center;
  border-radius: 15px;
  background-color: #9ac5e6;
  justify-content: space-between;
}

.card p {
  font-size: 15px;
  font-weight: bold;
  margin-bottom: 10px;
}

.Add,
.Edit,
.Delete {
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

.Btn-holder {
  gap: 10px;
  display: flex;
  align-items: center;
}

.checkbox {
  gap: 10px;
  display: flex;
  align-items: center;
}
.done {
  border: none;
  color: #f1f5f8;
  font-size: 20px;
  background-color: transparent;
}

.completed {
  text-decoration: line-through;
}
</style>
