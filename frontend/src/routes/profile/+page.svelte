<script lang="ts">
  import Flashes from '$lib/components/Flashes.svelte';
  import MessageModal from '$lib/components/MessageModal.svelte';
  import SubstitutableText from '$lib/components/SubstitutableText.svelte';
  import {i18n} from '$lib/i18n';

  import {invalidateAll} from '$app/navigation';

  import type {Data} from './+page';
  import {apiUpdateProfile} from './api';

  let {data}: {data: Data} = $props();

  let flashes: Flashes | undefined = $state();

  // Error handling
  let errorModal: {type: 'api-error'; message: string} | undefined = $state();

  function updateNewsOptIn(optIn: boolean): void {
    apiUpdateProfile({newsOptIn: optIn})
      .then((result) => {
        if (result.success) {
          invalidateAll();
        } else {
          errorModal = {
            type: 'api-error',
            message: `Failed to change news status: ${result.errorReason} (${result.errorDescription})`,
          };
        }
      })
      .catch((error) => {
        console.error('Profile update error', error);
        errorModal = {
          type: 'api-error',
          message: `Failed to change news status: ${error?.body?.message !== undefined ? error.body.message : error}`,
        };
      });
  }
</script>

{#if errorModal?.type === 'api-error'}
  <MessageModal
    type="error"
    title={$i18n.t('common.error--api-error')}
    message={errorModal.message}
    showClose={true}
    onClose={() => (errorModal = undefined)}
  />
{/if}

<nav class="breadcrumb" aria-label="breadcrumbs">
  <ul>
    <li><a href="/">{$i18n.t('navigation.home')}</a></li>
    <li class="is-active"><a href="./" aria-current="page">{$i18n.t('navigation.profile')}</a></li>
  </ul>
</nav>

<Flashes bind:this={flashes} />

<h2 class="title is-2">{$i18n.t('navigation.profile')}</h2>

<p class="block">
  <SubstitutableText text={$i18n.t('profile.prose--welcome')}>
    {#snippet snippet1()}<strong>{data.profile.username}</strong>{/snippet}
  </SubstitutableText>
</p>

<p class="block">
  <SubstitutableText text={$i18n.t('profile.prose--change-password')}>
    {#snippet snippet1(text)}<a href="/auth/password/change/">{text}</a>{/snippet}
  </SubstitutableText>
</p>

<div class="block">
  <h3 class="title is-4">{$i18n.t('profile.title--user-info')}</h3>
  <table class="table is-hoverable">
    <tbody>
      <tr>
        <th>{$i18n.t('profile.title--username')}</th>
        <td>{data.profile.username}</td>
      </tr>
      <tr>
        <th>{$i18n.t('profile.title--email')}</th>
        <td>{data.profile.email}</td>
      </tr>
      <tr>
        <th>{$i18n.t('profile.title--registered-since')}</th>
        <td>{data.profile.signedUp}</td>
      </tr>
    </tbody>
  </table>
</div>

<div class="block">
  <h3 class="title is-4">{$i18n.t('profile.title--news')}</h3>

  <article class="message is-info">
    <div class="message-body">
      {$i18n.t('profile.prose--news')}
    </div>
  </article>

  <div class="content">
    <p>
      {#if data.profile.newsOptIn}
        <SubstitutableText text={$i18n.t('profile.newsletter-allowed')}>
          {#snippet snippet1(text)}<strong>{text}</strong>{/snippet}
        </SubstitutableText>
      {:else}
        <SubstitutableText text={$i18n.t('profile.newsletter-disallowed')}>
          {#snippet snippet1(text)}<strong>{text}</strong>{/snippet}
        </SubstitutableText>{/if}
    </p>
  </div>

  <div class="block">
    <button
      class="button"
      class:is-warning={data.profile.newsOptIn}
      class:is-primary={!data.profile.newsOptIn}
      onclick={() => updateNewsOptIn(!data.profile.newsOptIn)}
    >
      {data.profile.newsOptIn
        ? $i18n.t('profile.newsletter-unsubscribe')
        : $i18n.t('profile.newsletter-subscribe')}
    </button>
  </div>
</div>
