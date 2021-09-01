<script>
  import Navbar from "./components/Navbar.svelte"
  import TodoInput from "./components/TodoInput.svelte";
  import TodoList from "./components/TodoList.svelte";

  let todos = [
    {id: 0, checked: false, text: "test1"},
    {id: 1, checked: true, text: "test2"}
  ]
  $: lastId = todos[todos.length - 1]?.id || 0;

  let todoInput = "";

  let onHandleKeyup = e => {
    todoInput = e.target.value;

    if (e.keyCode === 13) {
      onHandleAdd();
    }
  };
  let onHandleAdd = () => {
    if (!todoInput) {
      return;
    }

    const newTodo = {
      id: ++lastId,
      checked: false,
      text: todoInput,
    };

    todos = [...todos, newTodo];

    todoInput = "";
  };

  let onHandleCheck = (id) => {
    const index = todos.findIndex((todo) => todo.id === id);
    todos[index]["checked"] = !todos[index]["checked"];
  };

  let onHandleRemove = (id) => {
    todos = todos.filter((todo) => todo.id !== id);
  };

  let onHandleModify = (id, text) => {
    const index = todos.findIndex((todo) => todo.id === id);
    todos[index]["text"] = text;
  };
</script>

<section class="hero is-primary is-fullheight">
  <div class="hero-head">
    <Navbar/>
  </div>

  <div class="hero-body">
    <div class="container has-text-centered">
      <div class="columns is-desktop">
        <div class="column"></div>
        <div class="column">
          <h1 class="title">Todo List</h1>
          <h2 class="subtitle">Built with Svelte, Bulma</h2>
          <TodoInput {todoInput} {onHandleKeyup} {onHandleAdd}/>
          <TodoList {todos} {onHandleCheck} {onHandleRemove} {onHandleModify}/>
        </div>
        <div class="column"></div>
      </div>
    </div>
  </div>
</section>

<style lang="scss">
  main {
    text-align: center;

    & > h1 {
      color: #ff3e00
    }
  }
</style>