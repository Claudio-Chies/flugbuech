<script lang="ts">
  import {ensureError} from '$lib/assert';
  import Flashes from '$lib/components/Flashes.svelte';
  import MessageModal from '$lib/components/MessageModal.svelte';
  import SubstitutableText from '$lib/components/SubstitutableText.svelte';
  import {i18n} from '$lib/i18n';
  import {addFlash, refreshLoginState} from '$lib/stores';
  import {sanitizeRedirectPath} from '$lib/urls';

  import {goto} from '$app/navigation';
  import {page} from '$app/state';

  import {apiLogin} from './api';

  // Form values
  let username: string = $state('');
  let password: string = $state('');

  // Element bindings
  let flashes: Flashes | undefined = $state();

  // Error handling
  let submitEnabled = $state(true);
  let submitError: {type: 'api-error'; message: string} | undefined = $state();

  async function submitForm(): Promise<void> {
    submitEnabled = false;

    // Send login request to API
    let success;
    try {
      success = (await apiLogin(username, password)).success;
    } catch (error) {
      submitError = {
        type: 'api-error',
        message: ensureError(error).message,
      };
      submitEnabled = true;
      return;
    }

    if (success) {
      // Login successful! Add flash.
      addFlash({
        message: $i18n.t('auth.prose--logged-in'),
        severity: 'success',
        icon: 'fa-circle-check',
      });

      // Refresh login state
      refreshLoginState();

      // Redirect to home or to requested page
      goto(sanitizeRedirectPath(page.url.searchParams.get('redirect'), '/'));
    } else {
      // Login failed
      addFlash({
        message: $i18n.t('auth.error--login-failed'),
        severity: 'error',
        icon: 'fa-circle-exclamation',
      });
      flashes?.update(true);
      password = '';
    }

    submitEnabled = true;
  }
</script>

{#if submitError?.type === 'api-error'}
  <MessageModal
    type="error"
    title={$i18n.t('common.error--api-error')}
    message={submitError.message}
    showClose={true}
    onClose={() => (submitError = undefined)}
  />
{/if}

<Flashes bind:this={flashes} />

<h2 class="title is-size-2">{$i18n.t('navigation.login')}</h2>

<form
  method="post"
  onsubmit={(event) => {
    event.preventDefault();
    void submitForm();
  }}
>
  <div class="field">
    <label class="label" for="username">{$i18n.t('auth.title--username')}</label>
    <div class="control has-icons-left">
      <!-- svelte-ignore a11y_autofocus -->
      <input
        class="input"
        type="text"
        name="username"
        id="username"
        bind:value={username}
        required
        autofocus
      />
      <span class="icon is-small is-left">
        <i class="fas fa-user"></i>
      </span>
    </div>
  </div>
  <div class="field">
    <label class="label" for="password">{$i18n.t('auth.title--password')}</label>
    <div class="control has-icons-left">
      <input
        class="input"
        type="password"
        name="password"
        id="password"
        bind:value={password}
        required
      />
      <span class="icon is-small is-left">
        <i class="fas fa-lock"></i>
      </span>
    </div>
  </div>
  <div class="field">
    <div class="control">
      <button class="button is-primary" disabled={!submitEnabled} type="submit">
        {$i18n.t('navigation.login')}
      </button>
    </div>
  </div>

  <p>
    <SubstitutableText text={$i18n.t('auth.prose--register-now')}>
      {#snippet snippet1(text)}<a href="/auth/registration/">{text}</a>{/snippet}
    </SubstitutableText>
  </p>
</form>
