<!-- Map showing a single location. May be editable. -->
<script lang="ts">
  import BaseMap from './BaseMap.svelte';
  import type {NamedCoordinates} from './map';

  interface Props {
    markers: NamedCoordinates[];
  }

  const {markers}: Props = $props();
</script>

<BaseMap mode="multi" {markers} />
