<template>
  <div>
    <h1>By Zip Code</h1>
    <form @click.prevent="submit">
      <label for="zip">Zip Code</label>
      <input id="zip" name="zip" type="text" v-model="zip" />
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
      zip: "",
      locations: [],
    };
  },
  methods: {
    async submit() {
      const response = await this.$apollo.query({
        query: gql`
          query getByZip($zip: String!, $limit: Int, $radius: Int) {
            brand(slug: "titlemax") {
              locations(
                filterVirtualStores: true
                geoZip: { zip: $zip, limit: $limit, radius: $radius }
              ) {
                title
                link
                distance
                phone
              }
            }
          }
        `,
        variables: {
          zip: this.zip,
          limit: 25,
          radius: 50,
        },
      });

      this.locations = response.data?.brand?.locations || [];
    },
  },
};
</script>