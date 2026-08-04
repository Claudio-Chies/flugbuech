<script lang="ts">
  import {onMount} from 'svelte';

  import {requireLogin} from '$lib/auth';
  import Flashes from '$lib/components/Flashes.svelte';
  import SubstitutableText from '$lib/components/SubstitutableText.svelte';
  import {i18n} from '$lib/i18n';
  import {loginState} from '$lib/stores';

  import FlightForm from '../FlightForm.svelte';

  import type {Data} from './+page';

  let {data}: {data: Data} = $props();

  onMount(() => {
    requireLogin($loginState, '/flights/add/');
  });
</script>

<nav class="breadcrumb" aria-label="breadcrumbs">
  <ul>
    <li><a href="/">{$i18n.t('navigation.home')}</a></li>
    <li><a href="/flights/">{$i18n.t('navigation.flights')}</a></li>
    <li class="is-active">
      <a href="./" aria-current="page">{$i18n.t('flights.action--add-flight')}</a>
    </li>
  </ul>
</nav>

<Flashes />

<FlightForm
  gliders={data.gliders}
  lastGliderId={data.lastGliderId}
  locations={data.locations}
  existingFlightNumbers={data.existingFlightNumbers}
>
  {#snippet title()}
    <h2 class="title is-2">{$i18n.t('flight.title--add-flight')}</h2>
  {/snippet}

  {#snippet intro()}
    <section>
      <p class="content">
        {$i18n.t('flight.prose--fill-out-form')}
      </p>

      <p class="content">
        <SubstitutableText text={$i18n.t('flight.prose--note-add-location')}>
          {#snippet snippet1(text)}<strong>{text}</strong>{/snippet}
          {#snippet snippet2(text)}<a href="/locations/add/">{text}</a>{/snippet}
        </SubstitutableText>
      </p>

      {#if data.locations.length === 0}
        <article class="message is-warning">
          <div class="message-body">
            <i class="fa-solid fa-warning"></i>&ensp;<SubstitutableText
              text={$i18n.t('flight.prose--note-missing-location')}
            >
              {#snippet snippet1(text)}<strong>{text}</strong>{/snippet}
              {#snippet snippet2(text)}<a href="/locations/add/">{text}</a>{/snippet}
            </SubstitutableText>
          </div>
        </article>
      {/if}

      {#if data.gliders.length === 0}
        <article class="message is-warning">
          <div class="message-body">
            <i class="fa-solid fa-warning"></i>&ensp;<SubstitutableText
              text={$i18n.t('flight.prose--note-missing-gliders')}
            >
              {#snippet snippet1(text)}<strong>{text}</strong>{/snippet}
              {#snippet snippet2(text)}<a href="/gliders/">{text}</a>{/snippet}
            </SubstitutableText>
          </div>
        </article>
      {/if}
    </section>
  {/snippet}
</FlightForm>
