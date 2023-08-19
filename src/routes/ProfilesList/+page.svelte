<script>
	import { onMount } from 'svelte';

	import { ApolloClient, InMemoryCache, gql } from '@apollo/client';

	const client = new ApolloClient({
		uri: 'http://localhost:8080/v1/graphql',

		cache: new InMemoryCache()
	});

	/**
   * @type {any[]}
   */
	let profiles = [];

	const fetchprofiles = async () => {
		try {
			const { data } = await client.query({
				query: gql`
					query {
						profiles{
							id

							name

						}
					}
				`
			});

			// @ts-ignore
			profiles = data.profiles.map((e) => ({ ...e, editable: { ...e } }));
		} catch (error) {
			console.error('Error fetching profiles:', error);
		}
	};

	// @ts-ignore
	const updateprofile = async (id, name) => {
		try {
			const { data } = await client.mutate({
				mutation: gql`
					mutation Updateprofile($id: Int!, $name: String!) {
						update_profiles(
							where: { id: { _eq: $id } }
							_set: { name: $name}
						) {
							affected_rows
						}
					}
				`,

				variables: { id, name }
			});

			console.log('profile updated:', data);
		} catch (error) {
			console.error('Error updating profile:', error);
		}
	};

	// @ts-ignore
	const removeprofile = async (id) => {
		try {
			const { data } = await client.mutate({
				mutation: gql`
					mutation Removeprofile($id: Int!) {
						delete_profiles(where: { id: { _eq: $id } }) {
							affected_rows
						}
					}
				`,

				variables: { id }
			});

			console.log('profile deleted:', data);
		} catch (error) {
			console.error('Error deleting profile:', error);
		}
	};

	onMount(() => {
		fetchprofiles();
	});
</script>

<h1>employee Details:</h1>

{#each profiles as e (e.id)}
	<div>
		<input bind:value={e.editable.name} />

		<input bind:value={e.editable.id} />


		<button on:click={() => updateprofile(e.id, e.editable.name)}
			>Update</button
		>

		<button on:click={() => removeprofile(e.id)}>Delete</button>
	</div>
{/each}
