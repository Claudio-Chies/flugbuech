<script lang="ts">
  import {unreachable} from '$lib/assert';

  interface Props {
    href: string;
    text: string;
    type?: 'link' | 'button';
    reload?: boolean;
    closeMenu: () => void;
  }

  const {href, text, type = 'link', reload = false, closeMenu}: Props = $props();

  const linkClass = $derived.by(() => {
    switch (type) {
      case 'link':
        return 'navbar-item';
      case 'button':
        return 'button';
      default:
        return unreachable(type);
    }
  });
</script>

<a class={linkClass} {href} onclick={() => closeMenu()} data-sveltekit-reload={reload || null}>
  {text}
</a>
