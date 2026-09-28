<!-- @component
Render the XContest track type.
-->
<script lang="ts">
  import type {XContestTracktype} from '$lib/xcontest';

  interface Props {
    /**
     * The track type.
     */
    tracktype: XContestTracktype;
  }

  const {tracktype}: Props = $props();
</script>

<img
  src="/img/xcontest_{tracktype}.gif"
  alt={tracktype}
  title={tracktype}
  class="xcontest-tracktype"
/>

<style>
  .xcontest-tracktype {
    margin-bottom: -3px;
  }
</style>
