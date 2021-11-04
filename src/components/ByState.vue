<template>
  <div>
    <h1>By State</h1>
    <form @click.prevent="submit">
      <label for="state">State</label>
      <input id="state" name="state" type="text" v-model="state" />
      <button type="submit">Submit</button>
    </form>
    <pre>{{ JSON.stringify(locations, null, 2) }}</pre>
  </div>
</template>

<script>
import gql from "graphql-tag";

export default {
  data() {
    return {
      state: "GA",
      locations: [],
    };
  },
  methods: {
    async submit() {
      const response = await this.$apollo.query({
        query: gql`
          query getByState($state: String!) {
            brand(slug: "titlemax") {
              locations(state: $state) {
                title
                link
                phone
              }
            }
          }
        `,
        variables: {
          state: this.state,
          limit: 25,
          radius: 50,
        },
      });

      this.locations = response.data?.brand?.locations || [];
    },
  },
};
</script>