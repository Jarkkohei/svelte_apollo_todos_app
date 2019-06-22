<script>
	import ApolloClient from "apollo-boost";
	import { setClient } from "svelte-apollo";

	import Todos from "./Todos.svelte";

	const client = new ApolloClient({
	  uri: "https://todos-graphql.herokuapp.com/graphql"
	});
	setClient(client);
</script>

<style>
	main {
	  font-family: sans-serif;
	  text-align: center;
	}
</style>

<main>
	<h1>Svelte - Apollo Todos App</h1>
	<Todos />
</main>