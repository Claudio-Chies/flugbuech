<script lang="ts">
  import type {Snippet} from 'svelte';

  interface Props {
    type: 'warning' | 'error';
    title: string;
    message: string;
    showClose?: boolean;
    onClose?: () => void;
    buttons?: Snippet;
  }

  const {type, title, message, showClose = true, onClose, buttons}: Props = $props();

  const articleClass = $derived(type === 'warning' ? 'is-warning' : 'is-danger');
</script>

<div class="modal is-active">
  <div class="modal-background"></div>
  <div class="modal-content">
    <article class="message {articleClass}">
      <div class="message-header">
        <p>{title}</p>
        {#if showClose}
          <button class="delete" aria-label="close" onclick={() => onClose?.()}></button>
        {/if}
      </div>
      <div class="message-body" class:has-buttons={buttons}>
        <section>{message}</section>
        {@render buttons?.()}
      </div>
    </article>
  </div>
</div>

<style>
  /* TODO: Upgrade Bulma to 0.9+ and use the spacing helpers. */
  .message-body.has-buttons > section:first-of-type {
    margin-bottom: 16px;
  }
</style>
