<template>
  <div class="task-list-container">
    <h1>Laporan Kegiatan Hari Ini</h1>
    <input
      v-model="newTask"
      @keyup.enter="addTask"
      placeholder="Masukkan Laporan"
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
      Jumlah Laporan Hari Ini: {{ incompleteTasksCount }}
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
  background: linear-gradient(135deg, #ad3030, #e0e0e0);
  border-radius: 12px;
  box-shadow: 0 0 15px rgba(0, 0, 0, 0.2);
  font-family: "Arial", sans-serif;
}

/* Heading */
.task-list-container h1 {
  text-align: center;
  color: rgb(43, 36, 43);
  margin-bottom: 20px;
  font-size: 24px;
  font-weight: bold;
}

/* Input untuk menambah tugas baru */
.task-input {
  width: calc(100% - 120px);
  padding: 12px;
  margin-right: 10px;
  border: 2px solid #ccc;
  border-radius: 8px;
  box-sizing: border-box;
  font-size: 16px;
  transition: border-color 0.3s;
}

.task-input:focus {
  border-color: #a78528;
  outline: none;
}

/* Tombol tambah */
.add-button {
  padding: 12px 24px;
  background-color: brown;
  color: white;
  border: none;
  border-radius: 8px;
  cursor: pointer;
  font-size: 16px;
  transition: background-color 0.3s;
}

.add-button:hover {
  background-color: #884a21;
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
  padding: 12px;
  margin-bottom: 10px;
  background-color: #fff;
  border: 2px solid #ddd;
  border-radius: 8px;
  box-shadow: 0 0 5px rgba(0, 0, 0, 0.1);
  transition: transform 0.2s;
}

.task-item:hover {
  transform: scale(1.02);
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
  color: blue;
}

/* Tombol hapus */
.delete-button {
  padding: 6px 12px;
  background-color: brown;
  color: white;
  border: none;
  border-radius: 8px;
  cursor: pointer;
  font-size: 14px;
  transition: background-color 0.3s;
}

.delete-button:hover {
  background-color: #884a21;
}

/* Jumlah tugas yang belum selesai */
.incomplete-tasks-count {
  text-align: center;
  margin-top: 20px;
  font-size: 18px;
  color: rgb(43, 36, 43);
}
</style>
