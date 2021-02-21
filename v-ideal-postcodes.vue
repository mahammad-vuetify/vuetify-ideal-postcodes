<template>
  <v-autocomplete
    v-model="select"
    :loading="loading"
    :items="items"
    item-text="suggestion"
    item-value="udprn"
    :search-input.sync="search"
    cache-items
    class="mx-4"
    flat
    hide-no-data
    hide-details
    label="221B Baker Stre"
    solo-inverted
    @change="onChange()"
  ></v-autocomplete>
</template>

<script>
export default{
  data() {
    return {
      loading: false,
      items: [],
      search: null,
      select: null,
      result: null,
    };
  },
  watch: {
    search(val) {
      val && val !== this.select && this.querySelections(val);
    },
  },
  methods: {
    async querySelections(q) {
      this.loading = true;
      const response = await axios.get('https://api.ideal-postcodes.co.uk/v1/autocomplete/addresses', {
        params: {
          api_key: 'iddqd',
          q,
        },
      });

      if (response) {
        this.items = response.data.result.hits;
      }

      this.loading = false;
    },
    async onChange() {
      if (this.select) {
        const response = await axios.get(`https://api.ideal-postcodes.co.uk/v1/udprn/${this.select}`, {
          params: {
            api_key: 'iddqd',
          },
        });

        if (response) {
          this.result = response.data.result;
          this.$emit('change', this.result);
        }
      }
    },
  },
}
</script>
