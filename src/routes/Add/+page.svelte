<script>
  import { mutation } from "svelte-apollo";

  import { ApolloClient, InMemoryCache, gql } from "@apollo/client/core";

  import { setClient } from "svelte-apollo";

  const client = new ApolloClient({
    uri: "http://localhost:8080/v1/graphql",

    cache: new InMemoryCache(),
  });

  setClient(client);

  const signup_mutation = gql`
    mutation MyMutation($id: Int!, $name: String!) {
      insert_profiles(objects: { id: $id, name: $name }) {
        affected_rows
      }
    }
  `;

  const addprofile = mutation(signup_mutation);

  // @ts-ignore

  async function handleSubmit(event) {
    event.preventDefault();

    const data = new FormData(event.target);

    const id = data.get("id");

    const name = data.get("name");

    try {
      await addprofile({ variables: { id: id, name: name } });
    } catch (error) {
      console.error(error);
    }
  }
</script>

<div class="w-100 md:w-100 md:max-w-full mx-auto mt-5 ml-40 mr-40">
  <div class="p-6 border border-gray-300 sm:rounded-md">
    <form method="POST" on:submit|preventDefault={handleSubmit}>
      <label class="block mb-6">
        <span class="text-gray-700">Your name</span>

        <input
          type="text"
          name="name"
          class="

                block

                w-full

                mt-1

                border-gray-300

                rounded-md

                shadow-sm

                focus:border-indigo-300

                focus:ring

                focus:ring-indigo-200

                focus:ring-opacity-50

              "
          placeholder="Joe Bloggs"
        />
      </label>

      <label class="block mb-6">
        <span class="text-gray-700">id</span>

        <input
          name="id"
          type="text"
          class="

                block

                w-full

                mt-1

                border-gray-300

                rounded-md

                shadow-sm

                focus:border-indigo-300

                focus:ring

                focus:ring-indigo-200

                focus:ring-opacity-50

              "
          required
        />
      </label>

      <div class="mb-6">
        <button
          type="submit"
          class="

                h-10

                px-5

                text-indigo-100

                bg-indigo-700

                rounded-lg

                transition-colors

                duration-150

                focus:shadow-outline

                hover:bg-indigo-800

              "
        >
          Submit
        </button>
      </div>
    </form>
  </div>
</div>
