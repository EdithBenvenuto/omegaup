<template>
  <tags-input
    v-model="selectedOptions"
    :existing-tags="existingOptions"
    :typeahead="true"
    :typeahead-style="'dropdown'"
    :typeahead-max-results="maxResults"
    :typeahead-activation-threshold="activationThreshold"
    :placeholder="T.typeaheadSearchPlaceholder"
    :limit="1"
    :hide-input-on-limit="true"
    :only-existing-tags="true"
    :typeahead-hide-discard="true"
    @change="updateExistingOptions"
    @tag-added="onTagAdded"
    @tag-removed="onTagRemoved"
  >
  </tags-input>
</template>

<script lang="ts">
import { Vue, Component, Prop } from 'vue-property-decorator';
import VoerroTagsInput from '@voerro/vue-tagsinput';
import '@voerro/vue-tagsinput/dist/style.css';
import T from '../../lang';

@Component({
  components: {
    'tags-input': VoerroTagsInput,
  },
})
export default class Typeahead extends Vue {
  @Prop() existingOptions!: { key: string; value: string }[];
  @Prop({ default: 3 }) activationThreshold!: number;
  @Prop({ default: 5 }) maxResults!: number;

  T = T;
  selectedOptions: { key: string; value: string }[] = [];

  updateExistingOptions(query: string): void {
    if (query.length < this.activationThreshold) return;
    this.$emit('update-existing-options', query);
  }

  onTagAdded(): void {
    if (this.selectedOptions.length < 1) return;
    this.$emit('update-selected-option', this.selectedOptions[0].key);
  }

  onTagRemoved(): void {
    this.$emit('update-selected-option', null);
  }
}
</script>

<style lang="scss">
@import '../../../../sass/main.scss';

.tags-input-remove:before,
.tags-input-remove:after,
.tags-input-typeahead-item-highlighted-default {
  background-color: $omegaup-primary--darker;
}
</style>
