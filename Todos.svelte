<script>
  import { getClient, query, mutate } from "svelte-apollo";
  import { gql } from "apollo-boost";

  let id = "";
  let text = "";

  const GET_TODOS = gql`
    {
      getTodos {
        id
        text
        done
      }
    }
  `;

  const ADD_TODO = gql`
    mutation($text: String!) {
      addTodo(text: $text) {
        id
        done
        text
      }
    }
  `;

  const DELETE_TODO = gql`
    mutation($id: String!) {
      deleteTodo(id: $id) {
        id
        text
        done
      }
    }
  `;

  const TOGGLE_TODO = gql`
    mutation($id: String!) {
      toggleTodo(id: $id) {
        id
        text
        done
      }
    }
  `;

  const client = getClient();

  const todos = query(client, { query: GET_TODOS });

  function addTodo() {
    mutate(client, {
      mutation: ADD_TODO,
      variables: { text }
    })
      .then(() => {
        text = "";
        todos.refetch();
      })
      .catch(err => {
        console.log(err);
      });
  }

  function deleteTodo(todoId) {
    id = todoId;
    mutate(client, {
      mutation: DELETE_TODO,
      variables: { id }
    })
      .then(() => {
        id = "";
        todos.refetch();
      })
      .catch(err => {
        console.log(err);
      });
  }

  function toggleTodo(todoId) {
    id = todoId;
    mutate(client, {
      mutation: TOGGLE_TODO,
      variables: { id }
    })
      .then(() => {
        id = "";
        todos.refetch();
      })
      .catch(err => {
        console.log(err);
      });
  }
</script>

<style>
  ul {
    list-style: none;
  }

  .error {
    color: red;
  }

  .done {
    text-decoration: line-through;
  }

  .text {
    cursor: pointer;
  }

  button {
    cursor: pointer;
  }
</style>

<form on:submit|preventDefault={addTodo}>
  <input placeholder="Add a todo" bind:value={text}>
  <button>Submit</button>
</form>

<ul>
<h2>Todos</h2>
{#await $todos}
  Loading todos...
{:then result}
  <p>Total todos: {result.data.getTodos.length}</p>
  {#each result.data.getTodos as todo}
    <li class:done="{todo.done}">
      <span class="text" on:click={() => toggleTodo(todo.id)}>{todo.text}</span>
      <button on:click={() => deleteTodo(todo.id)}>X</button>
    </li>
  {/each}
{:catch error}
  <p class="error">{error}</p>
{/await}
</ul>