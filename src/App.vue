<template>
  <Navbar />

  <div class="container">
    <div class="row d-flex justify-content-center">
      <div class="col-sm-11 col-lg-8">
        <div class="row p-3 shadow my-3 d-flex justify-content-center">
          <div class="col-sm-11 col-lg-11">
            <div class="input-group my-3">
              <input
                v-model="todo"
                type="text"
                class="form-control bg-dark text-white"
                placeholder="Input Your Todo..."
                @keyup.enter="add"
              />
              <button class="btn btn-primary" type="button" @click="add">
                <i class="bi bi-plus-circle"></i> ADD
              </button>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>

  <Todo :todos="list" @del="del" @done="done" :total="total" />
</template>

<script>
import { ref, reactive, onMounted, toRefs, computed } from "vue";
import Navbar from "./components/Navbar.vue";
import Todo from "./components/Todo.vue";

export default {
  components: { Navbar, Todo },

  setup() {
    const todo = ref("");
    const todos = reactive({
      list: [],
    });

    onMounted(() => {
      const items = localStorage.getItem("VUE-COMPOSITION");
      todos.list = items ? JSON.parse(items) : [];
    });

    /**
     * Menghitung jumlah seluruh todo
     *
     * @returns {number}
     */
    const total = computed(() => {
      return todos.list.length;
    });

    /**
     * Menambahkan Todo
     *
     * @returns {void}
     */
    const add = () => {
      todos.list.unshift({ acticity: todo.value, isDone: false });
      todo.value = "";

      saveData();
    };

    /**
     * Menghapus todo yang telah di filter melalui index
     *
     * @param {number}
     * @returns {item}
     */
    const del = (indexTodo) => {
      todos.list = todos.list.filter((item, index) => {
        if (index !== indexTodo) {
          return item;
        }
      });
      saveData();
    };

    /**
     * Menandakan todo telah berhasil dilakukan
     *
     * @param {number}
     * @returns {item}
     */
    const done = (indexTodo) => {
      todos.list = todos.list.filter((item, index) => {
        if (index == indexTodo) {
          item.isDone = !item.isDone;
        }
        return item;
      });
      saveData();
    };

    /**
     * Mendaptarkan todo ke Local Storage
     *
     * @returns {void}
     */
    const saveData = () => {
      localStorage.setItem("VUE-COMPOSITION", JSON.stringify(todos.list));
    };

    return { total, todo, ...toRefs(todos), add, del, done };
  },
};
</script>
