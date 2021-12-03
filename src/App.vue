<template>
  <header class="header">
    <div class="container">
      <div class="header__title">TodoList</div>
      <form action="#" class="header__form" @submit.prevent="addTodo">
        <input
          class="header__form-item"
          type="text"
          placeholder="Todo title"
          required
          v-model="todoTitle"
        />
        <input
          class="header__form-item"
          type="text"
          placeholder="Todo subtitle"
          required
          v-model="todoSubtitle"
        />

        <div class="header__form-radios">
          <div class="header__form-radios-caption">
            <span>Priority</span>
          </div>

          <label class="header__form-radio" for="A"
            >A
            <input
              class="header__form-item"
              type="radio"
              name="priority"
              value="A"
              v-model="todoPriority"
              required
            />
          </label>

          <label class="header__form-radio" for="B"
            >B
            <input
              class="header__form-item"
              type="radio"
              name="priority"
              value="B"
              v-model="todoPriority"
            />
          </label>

          <label class="header__form-radio" for="C"
            >C
            <input
              class="header__form-item"
              type="radio"
              name="priority"
              value="C"
              v-model="todoPriority"
            />
          </label>
          <label class="header__form-radio" for="D"
            >D
            <input
              class="header__form-item"
              type="radio"
              name="priority"
              value="D"
              v-model="todoPriority"
            />
          </label>
        </div>

        <input class="form-button" type="submit" value="Add" />
      </form>
    </div>
  </header>

  <div class="sort">
    <div class="container">
      <div class="sort__wrap">
        <select
          name="sort__select"
          class="sort__items"
          :disabled="newTodos.length > 1 ? false : true"
          v-model="sortBy"
        >
          Sort by number
          <option
            name="sort-number"
            class="sort__item"
            value="Sort by number"
            selected
            disabled
          >
            Sort by number
          </option>
          <option
            name="sort-number"
            class="sort__item"
            value="From first to last"
          >
            From first to last
          </option>
          <option
            name="sort-number"
            class="sort__item"
            value="From last to first"
          >
            From last to first
          </option>
          <option name="sort-number" class="sort__item" value="From A to D">
            From A to D
          </option>
          <option name="sort-number" class="sort__item" value="From D to A">
            From D to A
          </option>
        </select>
      </div>
    </div>
  </div>

  <main class="main-content">
    <div class="container">
      <div class="main-content__wrap">
        <div class="main-content__row">
          <div class="main-content__col">
            <div class="main-content__title">New Todos</div>
            <div class="new-card" v-for="(item, i) in sortedTodos" :key="i">
              <div class="new-card__top">
                <div class="new-card__square new-card__id">
                  <span>{{ item.id }}</span>
                </div>
                <div class="new-card__square new-card__priority">
                  <span>{{ item.priority }}</span>
                </div>
              </div>
              <div class="new-card__title">{{ item.title }}</div>
              <div class="new-card__subtitle">
                {{ item.subtitle }}
              </div>
              <div class="new-card__button">
                <span @click="todoDone(i)">Done</span>
              </div>
            </div>
          </div>

          <div class="main-content__col">
            <div class="main-content__title">Old Todos</div>

            <div class="done-card" v-for="(item, i) in oldTodos" :key="i">
              <div class="done-card__top">
                <div class="done-card__square done-card__id">
                  <span>{{ item.id }}</span>
                </div>
                <div class="done-card__square done-card__priority">
                  <span>{{ item.priority }}</span>
                </div>
              </div>
              <div class="done-card__title">{{ item.title }}</div>
              <div class="done-card__subtitle">
                {{ item.subtitle }}
              </div>
              <div class="done-card__button done-card__button--repeat">
                <span @click="repeatTodo(i)">Repeat</span>
              </div>
              <div class="done-card__button">
                <span @click="deleteTodo(id)">Delete</span>
              </div>
            </div>
          </div>
        </div>

        <div class="main-content__row main-content__row--border-top">
          <div class="main-content__col">
            <div class="summary">
              <div class="summary__new-todos-title">Must to do:</div>
              <span class="summary__new-todos-count">
                {{ newTodos.length }}</span
              >
            </div>
          </div>

          <div class="main-content__col">
            <div class="summary">
              <div class="summary__done-todos-title">Were done:</div>
              <span class="summary__done-todos-count">
                {{ oldTodos.length }}
              </span>
            </div>
          </div>
        </div>
      </div>
    </div>
  </main>
</template>

<script>
import { ref, reactive, computed } from "vue";

export default {
  name: "App",

  setup() {
    let itemId = ref(0);
    let todoTitle = ref("");
    let todoSubtitle = ref("");
    let todoPriority = ref("");
    const sortBy = ref("Sort by number");

    const newTodos = reactive([]);
    const oldTodos = reactive([]);

    const addTodo = () => {
      itemId.value++;
      newTodos.unshift({
        id: itemId.value,
        title: todoTitle.value,
        subtitle: todoSubtitle.value,
        priority: todoPriority.value,
      });
      todoTitle.value = "";
      todoSubtitle.value = "";
      todoPriority.value = "";
    };
    const todoDone = (id) => {
      oldTodos.unshift(newTodos[id]);
      newTodos.splice(id, 1);
      console.log(id);
    };
    const repeatTodo = (id) => {
      newTodos.unshift(oldTodos[id]);
      oldTodos.splice(id, 1);
    };
    const deleteTodo = (id) => {
      oldTodos.splice(id, 1);
    };

    const sortedTodos = computed(() => {
      if (newTodos.length < 2) {
        sortBy.value = "Sort by number";
      }
      if (sortBy.value === "Sort by number") {
        return [...newTodos].sort((a, b) => a.id - b.id);
      }
      if (sortBy.value === "From first to last") {
        return [...newTodos].sort((a, b) => a.id - b.id);
      }
      if (sortBy.value === "From last to first") {
        return [...newTodos].sort((a, b) => b.id - a.id);
      }
      if (sortBy.value === "From A to D") {
        return [...newTodos].sort((a, b) =>
          a.priority.localeCompare(b.priority)
        );
      }
      if (sortBy.value === "From D to A") {
        return [...newTodos].sort((a, b) =>
          b.priority.localeCompare(a.priority)
        );
      }
      return newTodos;
    });
    return {
      itemId,
      todoTitle,
      todoSubtitle,
      todoPriority,
      sortBy,
      newTodos,
      oldTodos,
      addTodo,
      todoDone,
      repeatTodo,
      deleteTodo,
      sortedTodos,
    };
  },
};
</script>

<style>
</style>
