<template>
  <div>
    <h1>On Load</h1>
    <pre>{{ JSON.stringify(location, null, 2) }}</pre>
  </div>
</template>

<script>
import gql from "graphql-tag";

export default {
  data() {
    return {
      number: "17242",
      location: null,
    };
  },
  apollo: {
    location: {
      query: gql`
        query getByNumber($number: String!) {
          location(number: $number) {
            title
            link
            phone
            number
            latitude
            longitude
          }
        }
      `,
      variables() {
        return {
          number: this.number,
        };
      },
      update: (data) => data?.location || null,
    },
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