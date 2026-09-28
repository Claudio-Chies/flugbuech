<script lang="ts">
  import {onMount} from 'svelte';

  import Flashes from '$lib/components/Flashes.svelte';
  import SubstitutableText from '$lib/components/SubstitutableText.svelte';
  import {i18n} from '$lib/i18n';
  import {ResolvablePromise} from '$lib/resolvable-promise';

  import {_loadApiStats, type GlobalStats} from './+page';

  let stats = new ResolvablePromise<GlobalStats>();

  onMount(() => {
    // Load stats from API
    _loadApiStats().then((result) => stats.resolve(result));
  });
</script>

<Flashes />

<article class="message is-primary">
  <div class="message-header">
    <p>{$i18n.t('home.title--update-nov-2025')}</p>
  </div>
  <div class="message-body">
    <SubstitutableText text={$i18n.t('home.news.2025-11-14')}>
      {#snippet snippet1(text)}<a href="mailto:flugbuech@bargen.dev">{text}</a>{/snippet}
    </SubstitutableText>
  </div>
</article>

<h2 class="title is-size-2">{$i18n.t('home.title--overview')}</h2>

<p class="content">
  <SubstitutableText text={$i18n.t('home.prose--intro')}>
    {#snippet snippet1(text)}<em>{text}</em>{/snippet}
  </SubstitutableText>
</p>

<div class="content">
  <SubstitutableText text={$i18n.t('home.prose--what-is')}>
    {#snippet snippet1(text)}<em>{text}</em>{/snippet}
  </SubstitutableText>
  <ul>
    <li>
      <SubstitutableText text={$i18n.t('home.prose--what-is-summary')}>
        {#snippet snippet1(text)}<strong>{text}</strong>{/snippet}
      </SubstitutableText>
    </li>
    <li>
      <SubstitutableText text={$i18n.t('home.prose--what-is-goal')}>
        {#snippet snippet1(text)}<strong>{text}</strong>{/snippet}
      </SubstitutableText>
    </li>
    <li>
      <SubstitutableText text={$i18n.t('home.prose--what-is-additional-information')}>
        {#snippet snippet1(text)}<strong>{text}</strong>{/snippet}
      </SubstitutableText>
    </li>
    <li>
      <SubstitutableText text={$i18n.t('home.prose--what-is-stats')}>
        {#snippet snippet1(text)}<strong>{text}</strong>{/snippet}
      </SubstitutableText>
    </li>
    <li>
      <SubstitutableText text={$i18n.t('home.prose--what-is-no-ads')}>
        {#snippet snippet1(text)}<strong>{text}</strong>{/snippet}
      </SubstitutableText>
    </li>
  </ul>
</div>

<p class="content">
  <SubstitutableText text={$i18n.t('home.prose--screenshots')}>
    {#snippet snippet1(text)}<a href="/screenshots/">{text}</a>{/snippet}
    {#snippet snippet2(text)}<a href="https://github.com/dbrgn/flugbuech#status">{text}</a
      >{/snippet}
  </SubstitutableText>
</p>

<h3 class="title is-size-3">{$i18n.t('home.title--faq')}</h3>

<h4 class="title is-size-5">{$i18n.t('home.faq.is-it-free')}</h4>
<p class="content">
  {$i18n.t('home.faq.is-it-free-answer-1')}
</p>

<h4 class="title is-size-5">{$i18n.t('home.faq.signup')}</h4>
<p class="content">
  <SubstitutableText text={$i18n.t('home.faq.signup--answer-1')}>
    {#snippet snippet1(text)}<a href="/auth/registration/">{text}</a>{/snippet}
  </SubstitutableText>
</p>
<p class="content">
  <SubstitutableText text={$i18n.t('home.faq.signup--answer-2')}>
    {#snippet snippet1(text)}<a href="mailto:flugbuech@bargen.dev">{text}</a>{/snippet}
  </SubstitutableText>
</p>

<h4 class="title is-size-5">{$i18n.t('home.faq.import')}</h4>
<p class="content">
  <SubstitutableText text={$i18n.t('home.faq.import--answer-1')}>
    {#snippet snippet1(text)}<a href="/flights/import/csv/">{text}</a>{/snippet}
  </SubstitutableText>
</p>

<h4 class="title is-size-5">
  {$i18n.t('home.faq.source')}
</h4>
<p class="content">
  <SubstitutableText text={$i18n.t('home.faq.source-answer-1')}>
    {#snippet snippet1(text)}<a href="https://github.com/dbrgn/flugbuech">{text}</a>{/snippet}
    {#snippet snippet2(text)}<a href="mailto:flugbuech@bargen.dev">{text}</a>{/snippet}
  </SubstitutableText>
</p>

<h4 class="title is-size-5">{$i18n.t('home.faq.name')}</h4>
<p class="content">
  <SubstitutableText text={$i18n.t('home.faq.name-answer-1')}>
    {#snippet snippet1(text)}<a href="https://en.wikipedia.org/wiki/Swiss_German">{text}</a
      >{/snippet}
  </SubstitutableText>
</p>

<h3 class="title is-size-3">{$i18n.t('home.title--news')}</h3>

<h4 class="title is-size-5">2025</h4>
<ul class="content">
  <li>
    <strong>14.11.</strong>
    {$i18n.t('home.news.2025-11-14')}
  </li>
  <li>
    <strong>23.8.</strong>
    {$i18n.t('home.news.2025-08-23')}
  </li>
</ul>

<h4 class="title is-size-5">2024</h4>
<ul class="content">
  <li>
    <strong>25.5.</strong>
    {$i18n.t('home.news.2024-05-25')}
  </li>
  <li>
    <strong>14.5.</strong>
    <SubstitutableText text={$i18n.t('home.news.2024-05-14')}>
      {#snippet snippet1(text)}<a href="/locations/">{text}</a>{/snippet}
    </SubstitutableText>
  </li>
  <li>
    <strong>1.4.</strong>
    <SubstitutableText text={$i18n.t('home.news.2024-04-01')}>
      {#snippet snippet1(text)}<a href="/flights/import/csv/">{text}</a>{/snippet}
    </SubstitutableText>
  </li>
  <li>
    <strong>19.3.</strong>
    <SubstitutableText text={$i18n.t('home.news.2024-03-19')}>
      {#snippet snippet1(text)}<a href="/profile/">{text}</a>{/snippet}
    </SubstitutableText>
  </li>
  <li>
    <strong>13.3.</strong>
    <SubstitutableText text={$i18n.t('home.news.2024-03-13')}>
      {#snippet snippet1(text)}<a href="mailto:flugbuech@bargen.dev">{text}</a>{/snippet}
    </SubstitutableText>
  </li>
</ul>

<h4 class="title is-size-5">2019­–2023</h4>
<ul class="content">
  <li>
    <strong>2023</strong>
    {$i18n.t('home.news.2023')}
  </li>
  <li>
    <strong>2021</strong>
    {$i18n.t('home.news.2021')}
  </li>
  <li>
    <strong>2020</strong>
    {$i18n.t('home.news.2020')}
  </li>
  <li>
    <strong>2019</strong>
    {$i18n.t('home.news.2019')}
  </li>
</ul>

<h3 class="title is-size-3">Stats</h3>
<table class="table is-bordered is-hoverable">
  <tbody>
    <tr>
      <th>Registered Users</th>
      <td>
        {#await stats}
          Loading...
        {:then result}
          {result.userCount}
        {/await}
      </td>
    </tr>
    <tr>
      <th>Registered Gliders</th>
      <td>
        {#await stats}
          Loading...
        {:then result}
          {result.gliderCount}
        {/await}
      </td>
    </tr>
    <tr>
      <th>Total Flights</th>
      <td>
        {#await stats}
          Loading...
        {:then result}
          {result.flightCount}
        {/await}
      </td>
    </tr>
  </tbody>
</table>
