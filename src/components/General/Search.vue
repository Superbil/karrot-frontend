<template>
  <div class="wrapper">
    <q-search
      :autofocus="true"
      separator
      class="lightgrey"
      v-model="terms"
      :placeholder="$t('BUTTON.SEARCH')"
      @blur="hideIfEmpty"
    >
      <q-autocomplete
        @search="search"
        @selected="selected"
      />
    </q-search>
  </div>
</template>

<script>
import { QSearch, QAutocomplete } from 'quasar'
import { mapActions, mapGetters } from 'vuex'

export default {
  components: { QSearch, QAutocomplete },
  data () {
    return {
      terms: null,
    }
  },
  methods: {
    ...mapActions({
      setTerms: 'search/setTerms',
      hide: 'search/hide',
      hideIfEmpty: 'search/hideIfEmpty',
    }),
    search (terms, done) {
      if (!terms) done([])
      this.setTerms(terms)
      setTimeout(() => done(this.results), 50)
    },
    selected (item) {
      this.terms = item.label
      this.hide()
      this.$router.push(item.value)
    },
  },
  computed: {
    ...mapGetters({
      results: 'search/results',
    }),
  },
}
</script>

<style scoped lang="stylus">
.lightgrey
  background-color lightgrey
</style>
