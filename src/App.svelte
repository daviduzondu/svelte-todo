<script>
  import Item from "./components/Item.svelte";
  let todo = JSON.parse(localStorage.getItem("todo")) || [];
  let errorMessage = "Item cannot be empty!";
  let duplicateMessage = "Item already added!";
  let successMessage = "Item has been added and list updated!";
  let alertMessage;
  function validate(item) {
    let allItems = todo.map((x) => x.item);
    if (item.length === 0) {
      return "error";
    } else if (allItems.includes(item.trimEnd().trimStart())) {
      return "duplicate";
    } else {
      return "success";
    }
  }

  function displayAlert(type) {
    let alert = document.querySelector(".alert");
    alert.classList.remove("hidden");
    if (type === "error") {
      alertMessage = errorMessage;
      alert.classList.add("error");
    } else if (type === "duplicate") {
      alert.classList.add("error");
      alertMessage = duplicateMessage;
    } else {
      alert.classList.add("success");
      alertMessage = successMessage;
    }
    setTimeout(() => {
      alert.className = "alert hidden";
    }, 2000);
  }

  function addTodo(item) {
    let response = validate(item);
    if (response === "error") {
      displayAlert("error");
    } else if (response === "duplicate") {
      displayAlert("duplicate");
    } else {
      todo = [{ item: item.trimEnd().trimStart(), done: false }, ...todo];
      console.log(todo);
      localStorage.setItem("todo", JSON.stringify(todo));
      document.querySelector("input").value = "";
      displayAlert("success");
    }
  }

  function updateTodo(item) {
    let target = todo.findIndex((x) => x.item === item);
    todo[target].done = !todo[target].done;
    localStorage.setItem("todo", JSON.stringify(todo));
  }
  
  function removeTodo(item) {
    todo = todo.filter((x) => x.item !== item);
    localStorage.setItem("todo", JSON.stringify(todo));
  }
</script>

<main>
  <div class="app">
    <div>
      <h6 class="alert hidden">{alertMessage}</h6>
      <h1>Svelte To-Do List</h1>
      <form
        on:submit|preventDefault={() =>
          addTodo(document.querySelector("input").value)}
      >
        <div>
          <input type="text" placeholder="Enter Something" maxlength="39" />
          <button type="submit">Add</button>
        </div>
      </form>
    </div>
    <div class="todo-container">
      {#if todo.length}
        <div class="todo">
          {#each todo as { item, done } (item)}
            <Item
              {item}
              {done}
              removeTodoHandler={removeTodo}
              updateTodoHandler={updateTodo}
            />
          {/each}
        </div>
      {:else}
        <div>[Empty]</div>
      {/if}
    </div>
  </div>
</main>

<style>
  :global(.alert) {
    padding: 10px;
    color: white;
    position: absolute;
    left: 50%;
    top: -25px;
    transform: translateX(-50%);
    width: max-content;
  }
  :global(.error) {
    background-color: rgba(255, 0, 0, 0.6);
  }
  :global(.success) {
    background-color: rgba(0, 128, 0, 0.6);
  }
  :global(.hidden) {
    display: none;
  }
</style>
