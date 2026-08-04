<!-- @component
Render a XContest flight summary, with icon, distance and link.
-->
<script lang="ts">
  import {formatDistance} from '$lib/formatters';
  import {i18n} from '$lib/i18n';
  import {tracktypeName, type XContestTracktype} from '$lib/xcontest';

  import XContestTracktypeIcon from './XContestTracktypeIcon.svelte';

  interface Props {
    /**
     * The track type.
     */
    tracktype: XContestTracktype | undefined;

    /**
     * The flight distance in km.
     */
    distance: number | undefined;

    /**
     * The XContest URL.
     */
    url: string | undefined;

    /**
     * Whether to format the link to XContest in subtle colors.
     */
    subtleLink?: boolean;
  }

  const {tracktype, distance, url, subtleLink = false}: Props = $props();
</script>

{#if tracktype === undefined && distance === undefined && url === undefined}
  -
{:else if url}
  <a class:subtle-link={subtleLink} href={url}>
    {#if tracktype}
      <XContestTracktypeIcon {tracktype} />
    {/if}
    {#if distance}
      {formatDistance(distance)}
    {:else if tracktype}
      {tracktypeName(tracktype)}
    {:else}
      {$i18n.t('common.flight')}
    {/if}
  </a>
{:else}
  {#if tracktype}
    <XContestTracktypeIcon {tracktype} />
  {/if}
  {#if distance}
    {formatDistance(distance)}
  {:else if tracktype}
    {tracktypeName(tracktype)}
  {:else}
    {$i18n.t('common.flight')}
  {/if}
{/if}
