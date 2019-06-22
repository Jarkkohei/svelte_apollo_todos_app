<script>
	import ApolloClient from "apollo-boost";
	import { setClient } from "svelte-apollo";

	const client = new ApolloClient({
	  uri: "https://todos-graphql.herokuapp.com/graphql"
	});
	setClient(client);
</script>

<style>
</style>

<main>
	<h1>Svelte - Apollo Todos App</h1>

</main>