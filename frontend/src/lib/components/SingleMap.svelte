<!-- Map showing a single location. May be editable. -->
<script lang="ts">
  import type {LngLatLike} from 'maplibre-gl';

  import BaseMap from './BaseMap.svelte';
  import {DEFAULT_MAP_CENTER} from './map';

  interface Props {
    latitude?: number | null;
    longitude?: number | null;
    editable?: boolean;
    center?: LngLatLike;
    zoom?: number;
    // Callback props for location data lookups (forwarded to BaseMap)
    onElevationLookup?:
      | ((data: {elevation: number | null; zoomTooLow: boolean}) => void)
      | undefined;
    onCountryLookup?: ((data: {countryCode: string | null}) => void) | undefined;
  }

  let {
    latitude = $bindable(null),
    longitude = $bindable(null),
    editable = false,
    center = DEFAULT_MAP_CENTER,
    zoom = 6,
    onElevationLookup = undefined,
    onCountryLookup = undefined,
  }: Props = $props();
</script>

<BaseMap
  mode="single"
  bind:latitude
  bind:longitude
  {editable}
  {center}
  {zoom}
  {onElevationLookup}
  {onCountryLookup}
/>
