<template>
  <el-container class="container">
    <el-header>
      <h1>我的待办事项列表</h1>
    </el-header>

    <el-main>
      <el-input 
        v-model="newTask" 
        placeholder="需要做什么?" 
        class="task-input"
      ></el-input>
      <el-button 
        type="primary" 
        @click="addTask" 
        class="add-button"
        :disabled="isAddButtonDisabled"
      >添加</el-button>

      <p class="status">{{ completedTasksCount }} 个任务已完成，总共 {{ tasks.length }} 个任务</p>

      <el-list>
        <el-list-item 
          v-for="(task, index) in tasks" 
          :key="task.id"
          class="task-item"
        >
          <el-checkbox 
            v-model="task.completed" 
            class="task-checkbox"
          ></el-checkbox>
          <span :class="{ completed: task.completed }">{{ task.name }}</span>
          <div>
            <el-button 
              size="small" 
              @click="editTask(index)" 
              class="edit-button"
              aria-label="编辑任务"
            >编辑</el-button>
            <el-button 
              size="small" 
              type="danger" 
              @click="deleteTask(index)"
              aria-label="删除任务"
            >删除</el-button>
          </div>
        </el-list-item>
      </el-list>
    </el-main>
  </el-container>
</template>

<script>
let nextId = 1; // 用于跟踪 ID

export default {
  data() {
    return {
      newTask: '',
      tasks: [
        { id: nextId++, name: '学习 Vue', completed: false },
        { id: nextId++, name: '使用 CLI 创建 Vue 项目', completed: false },
        { id: nextId++, name: '享受乐趣', completed: false },
        { id: nextId++, name: '创建待办列表', completed: false }
      ]
    };
  },
  computed: {
    completedTasksCount() {
      return this.tasks.filter(task => task.completed).length;
    },
    isAddButtonDisabled() {
      return this.newTask.trim() === '' || this.tasks.some(task => task.name === this.newTask.trim());
    }
  },
  methods: {
    addTask() {
      if (!this.isAddButtonDisabled) {
        this.tasks.push({
          id: nextId++,
          name: this.newTask.trim(),
          completed: false
        });
        this.newTask = '';
      }
    },
    editTask(index) {
      const newName = prompt('编辑任务', this.tasks[index].name);
      if (newName !== null && newName.trim() !== '' && newName.trim() !== this.tasks[index].name) {
        this.tasks[index].name = newName.trim();
      }
    },
    deleteTask(index) {
      this.tasks.splice(index, 1);
    }
  }
};
</script>

<style scoped>
.container {
  max-width: 100%;
  margin: auto;
  padding: 20px;
  border: 1px solid #ccc;
  border-radius: 5px;
  background-color: #f9f9f9;
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
}

.task-input {
  margin-bottom: 10px;
  width: 100%;
}

.add-button {
  margin-bottom: 20px;
}

.status {
  margin: 10px 0;
  font-weight: bold;
}

.task-item {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 10px 0;
  border-bottom: 1px solid #eaeaea;
}

.task-checkbox {
  margin-right: 10px;
}

.completed {
  color: #999;
  opacity: 0.7; /* 增加美观度 */
}

.edit-button {
  margin-left: 10px;
}
</style>