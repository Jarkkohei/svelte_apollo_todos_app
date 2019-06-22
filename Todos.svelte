<script>
  import { getClient, query } from "svelte-apollo";
  import { gql } from "apollo-boost";

  const GET_TODOS = gql`
    {
      getTodos {
        id
        text
        done
      }
    }
  `;

  const client = getClient();

  const todos = query(client, { query: GET_TODOS });
</script>

<style>
  ul {
    list-style: none;
  }

  .error {
    color: red;
  }
</style>

<ul>
<h2>Todos</h2>
{#await $todos}
  Loading todos...
{:then result}
  <p>Total todos: {result.data.getTodos.length}</p>
  {#each result.data.getTodos as todo, index}
    <li>
      {todo.text}
    </li>
  {/each}
{:catch error}
  <p class="error">{error}</p>
{/await}
</ul>