<template>
  <br-meta :meta="component.getMeta()">
    <slot>
      <br-node-container-item v-if="isContainerItem(component)" :container-item="component" />

      <br-node-container v-else-if="isContainer(component)" :container="component">
        <br-node-component
          v-for="(component, key) in component.getChildren()" :key="key"
          :component="component" />
      </br-node-container>

      <br-node-component
        v-else
        v-for="(component, key) in component.getChildren()" :key="key"
        :component="component" />
    </slot>
  </br-meta>
</template>

<script lang="ts">
import { Component as SpaComponent, isContainerItem, isContainer } from '@bloomreach/spa-sdk';
import { Component, Prop, Provide, Vue } from 'vue-property-decorator';
import BrMeta from '../BrMeta.vue';
import BrNodeContainerItem from './BrNodeContainerItem.vue';
import BrNodeContainer from './BrNodeContainer.vue';

@Component({
  components: {
    BrMeta,
    BrNodeContainerItem,
    BrNodeContainer,
  },
  methods: {
    isContainerItem,
    isContainer,
  },
  name: 'br-node-component',
})
export default class BrNodeComponent extends Vue {
  @Prop() component!: SpaComponent;

  @Provide() // ProvideReactive doesn't work for recursive components
  private $component() {
    return this.component;
  }
}
</script>
