<template>
  <div>
    <h1>By Latitude / Longitude</h1>
    <form @click.prevent="submit">
      <div>
        <label for="latitude">Latitude</label>
        <input id="latitude" name="latitude" type="text" v-model="latitude" />
      </div>
      <div>
        <label for="longitude">Longitude</label>
        <input
          id="longitude"
          name="longitude"
          type="text"
          v-model="longitude"
        />
      </div>
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
      latitude: 32.026223,
      longitude: -81.1095689,
      locations: [],
    };
  },
  methods: {
    async submit() {
      const response = await this.$apollo.query({
        query: gql`
          query getByCoords(
            $lat: Float!
            $lng: Float!
            $limit: Int
            $radius: Int
          ) {
            brand(slug: "titlemax") {
              locations(
                filterVirtualStores: true
                geoCoords: {
                  lat: $lat
                  lng: $lng
                  limit: $limit
                  radius: $radius
                }
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
          lat: this.latitude,
          lng: this.longitude,
          limit: 25,
          radius: 50,
        },
      });

      this.locations = response.data?.brand?.locations || [];
    },
  },
};
</script>