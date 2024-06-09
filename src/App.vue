<template>
  <div class="task-list-container">
    <h1>Tujuan Dan Pencapaian</h1>
    <input
      v-model="newTask"
      @keyup.enter="addTask"
      placeholder="Tambah tujuan"
      class="task-input"
    />
    <button @click="addTask" class="add-button">Tambah</button>
    <ul class="task-list">
      <li v-for="task in tasks" :key="task.id" class="task-item">
        <input
          type="checkbox"
          :checked="task.completed"
          @change="toggleTaskCompletion(task.id)"
          class="task-checkbox"
        />
        <span
          :class="{ completed: task.completed, 'task-text': true }"
          @click="toggleTaskCompletion(task.id)"
        >
          {{ task.text }}
        </span>
        <button @click="removeTask(task.id)" class="delete-button">
          Hapus
        </button>
      </li>
    </ul>
    <p class="incomplete-tasks-count">
      Tujuan yang belum tercapai: {{ incompleteTasksCount }}
    </p>
  </div>
</template>

<script>
import { ref, computed } from "vue";
import { useTaskStore } from "./stores/todo";

export default {
  setup() {
    const taskStore = useTaskStore();
    const newTask = ref("");

    const addTask = () => {
      if (newTask.value.trim()) {
        taskStore.addTask(newTask.value);
        newTask.value = "";
      }
    };

    const removeTask = (taskId) => {
      taskStore.removeTask(taskId);
    };

    const toggleTaskCompletion = (taskId) => {
      taskStore.toggleTaskCompletion(taskId);
    };

    const incompleteTasksCount = computed(() => taskStore.incompleteTasksCount);

    return {
      newTask,
      tasks: taskStore.tasks,
      incompleteTasksCount,
      addTask,
      removeTask,
      toggleTaskCompletion,
    };
  },
};
</script>

<style>
/* Container utama untuk daftar tugas */
.task-list-container {
  max-width: 600px;
  margin: 0 auto;
  padding: 20px;
  background-color: #f9f9f9;
  border-radius: 8px;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
  font-family: "Arial", sans-serif;
}

/* Heading */
.task-list-container h1 {
  text-align: center;
  color: #333;
  margin-bottom: 20px;
}

/* Input untuk menambah tugas baru */
.task-input {
  width: calc(100% - 100px);
  padding: 10px;
  margin-right: 10px;
  border: 1px solid #ccc;
  border-radius: 4px;
  box-sizing: border-box;
  font-size: 16px;
}

/* Tombol tambah */
.add-button {
  padding: 10px 20px;
  background-color: #28a745;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  font-size: 16px;
}

.add-button:hover {
  background-color: #218838;
}

/* Daftar tugas */
.task-list {
  list-style-type: none;
  padding: 0;
  margin: 20px 0;
}

/* Item tugas */
.task-item {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 10px;
  margin-bottom: 10px;
  background-color: #fff;
  border: 1px solid #ddd;
  border-radius: 4px;
  box-shadow: 0 0 5px rgba(0, 0, 0, 0.05);
}

/* Checkbox tugas */
.task-checkbox {
  margin-right: 10px;
}

/* Teks tugas */
.task-text {
  flex: 1;
  cursor: pointer;
  font-size: 16px;
}

/* Tugas yang sudah selesai */
.completed {
  text-decoration: line-through;
  color: #777;
}

/* Tombol hapus */
.delete-button {
  padding: 5px 10px;
  background-color: #dc3545;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}

.delete-button:hover {
  background-color: #c82333;
}

/* Jumlah tugas yang belum selesai */
.incomplete-tasks-count {
  text-align: center;
  margin-top: 20px;
  font-size: 18px;
  color: #333;
}
</style>
