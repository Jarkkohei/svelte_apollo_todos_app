<script>
  import { getClient, query, mutate } from "svelte-apollo";
  import { gql } from "apollo-boost";

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
  {#each result.data.getTodos as todo, index}
    <li class:done="{todo.done}">
      {todo.text}
    </li>
  {/each}
{:catch error}
  <p class="error">{error}</p>
{/await}
</ul>