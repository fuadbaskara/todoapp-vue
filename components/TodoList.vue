<template>
<div>
  <div class="input-container">
    <input type="text" class="todo-input" v-model="todo" v-on:keyup.enter="addTodo" placeholder="What to do..">
    <button type="button" class="submit-button pointer" v-on:click="addTodo()" name="button">SUBMIT</button>
  </div>
  <div class="todos-container">
    <div v-if="todoList.length !== 0" class="inner-container">
      <div class="todo-card" v-for="(todo, index) in todoList" :key="index">
        <div v-if="!todo.updateState" class="todo-desc pointer" v-on:dblclick="editTodo(todo)">{{ todo.todo }}</div>
        <input v-else-if="!todo.completed" type="text" class="edit-todo" v-focus v-model="todo.todo"
          v-on:keyup.enter="updateTodo(todo.id, todo)" v-on:keyup.esc="cancelEdit(todo)"
          v-on:blur="updateTodo(todo.id, todo)" />
        <div>
          <div class="delete-button pointer" v-on:click="deleteTodo(index, todo.id)" :key="index">
            x </div>
        </div>
      </div>
    </div>
    <div v-else-if="loading === false && todoList.length === 0" class="placeholder">
      <div class="todo-placeholder">
        <p>There's nothing todo yet, Yay!</p>
      </div>
    </div>
    <div v-else-if="loading === null" class="placeholder">
      <div class="todo-placeholder">
        <p>Keep calm and sit tight :)</p>
      </div>
    </div>
  </div>
</div>
</template>

<script>
import axios from "axios";

export default {
  name: "todo-list",
  data() {
    return {
      loading: null,
      isEmpty: false,
      cachedTodo: "",
      todo: "",
      todoList: []
    };
  },
  directives: {
    focus: {
      inserted: function(el) {
        el.focus();
      }
    }
  },
  mounted() {
    this.loading = true;
    axios
      .get("http://localhost:3000/todoList")
      .then(response => {
        let data = [];
        for (var i = 0; i < response.data.length; i++) {
          data.push({
            id: response.data[i].id,
            todo: response.data[i].todo,
            updateState: false,
            completed: false
          });
        }
        this.todoList = data;
      })
      .catch(function(error) {
        console.log(error);
      });
    this.loading = false;
  },
  methods: {
    deleteTodo(index, id) {
      let newTodoList = this.todoList;
      newTodoList.splice(index, 1);
      axios
        .delete(`http://localhost:3000/todoList/${id}`, {
          data: {
            id: id
          }
        })
        .catch(function(error) {
          console.log(error);
        });
    },
    addTodo() {
      if (this.todo !== "") {
        let newId = this.generateId();
        this.todoList.push({
          id: newId,
          todo: this.todo
        });
        axios.post("http://localhost:3000/todoList", {
          id: newId,
          todo: this.todo
        });
        this.todo = "";
      } else {
        alert("input can't be empty");
      }
    },
    generateId() {
      let newId = "";
      for (let i = 0; i < 5; i++) {
        newId = newId + Math.floor(Math.random() * 9 + 1);
      }
      return newId;
    },
    async updateTodo(id, todo) {
      await axios
        .put(`http://localhost:3000/todoList/${id}`, {
          todo: todo.todo
        })
        .catch(function(error) {
          console.log(error);
        });
      todo.updateState = false;
    },
    editTodo(todo) {
      this.cachedTodo = todo.todo;
      todo.updateState = true;
    },
    cancelEdit(todo) {
      todo.todo = this.cachedTodo;
      todo.updateState = false;
    }
  }
};
</script>

<style lang="scss">
.todos-container {
  min-height: 350px;
  max-height: 350px;
  overflow: auto;
}
.todo-card {
  display: flex;
  justify-content: space-between;
  border-radius: 8px;
  box-shadow: 0.5px 0.9px 5px 0 rgba(0, 0, 0, 0.5);
  padding: 8px;
  margin: 10px;
}
.placeholder {
  width: 100%;
  height: 100%;
}
.todo-placeholder {
  margin-top: 35%;
}
.todo-placeholder p {
  color: rgb(210, 204, 204);
  font-size: 28px;
}
.todo-input {
  width: 100%;
  padding: 8px;
  outline: 0;
  margin-right: 5px;
  font-size: 16px;
  border: 1px solid #55cf61;
  border-radius: 8px;
}
.pointer {
  cursor: pointer;
}
.delete-button {
  width: 18px;
  height: 18px;
  font-size: 12px;
  font-weight: bolder;
  color: #d05959;
  border-radius: 50%;
  border: 1px solid #d05959;
  transition: 0.2s;
  &:hover {
    transition: 0.2s;
    background: #d05959;
    color: white;
  }
}
.edit-todo {
  border: none;
  border-bottom: 1px solid #55cf61;
  font-size: 16px;
  outline: 0;
  height: 18px;
}
.submit-button {
  border: 1px solid #55cf61;
  outline: 0;
  background: #55cf61;
  border-radius: 8px;
  font-weight: bolder;
  color: white;
  transition: 0.3s;
  &:hover {
    transition: 0.3s;
    background: #429f4b;
  }
  &:active {
    transition: 0.2s;
    box-shadow: 0.5px 0.9px 5px 0 rgb(140, 219, 103);
  }
}
.input-container {
  display: flex;
  justify-content: space-around;
}
.todos-container {
  margin: 15px 0 0;
  border-radius: 8px;
  box-shadow: 0.5px 0.9px 5px 0 rgba(0, 0, 0, 0.5);
  width: 100%;
}
.inner-container {
  padding: 10px;
}
</style>
