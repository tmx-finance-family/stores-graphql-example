<template>
  <div>
    <h1>By Store Number</h1>
    <form @click.prevent="submit">
      <label for="number">Store Number</label>
      <input id="number" name="number" type="text" v-model="number" />
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
      number: "17242",
      locations: [],
    };
  },
  methods: {
    async submit() {
      const response = await this.$apollo.query({
        query: gql`
          query getByNumber($number: String!) {
            brand(slug: "titlemax") {
              locations(number: $number) {
                title
                link
                phone
              }
            }
          }
        `,
        variables: {
          number: this.number,
          limit: 25,
          radius: 50,
        },
      });

      this.locations = response.data?.brand?.locations || [];
    },
  },
};
</script>