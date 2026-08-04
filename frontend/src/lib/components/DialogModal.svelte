<script lang="ts">
  import type {Snippet} from 'svelte';

  interface Props {
    title: string;
    message: string;
    /**
     * The BulmaCSS class.
     */
    dialogClass?: 'is-link' | 'is-warning' | 'is-danger';
    buttons?: Snippet;
  }

  const {title, message, dialogClass = 'is-link', buttons}: Props = $props();
</script>

<div class="modal is-active">
  <div class="modal-background"></div>
  <div class="modal-content">
    <article class="message {dialogClass}">
      <div class="message-header">
        <p>{title}</p>
      </div>
      <div class="message-body">
        <section class="dialog-text">{message}</section>
        <section class="dialog-buttons">
          {@render buttons?.()}
        </section>
      </div>
    </article>
  </div>
</div>

<style>
  /* TODO: Upgrade Bulma to 0.9+ and use the spacing helpers. */
  .dialog-buttons {
    display: flex;
    justify-content: flex-end;
    margin-top: 16px;
  }
</style>
