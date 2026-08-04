<!--
    @component Render text where a placeholder can be replaced with a snippet.

    Example with a placeholder being replaced with <br>:

        <SubstitutableText text="Hello<1/>World">
            {#snippet snippet1()}<br />{/snippet}
        </SubstitutableText>

    Example with a placeholder wrapped in an <a> tag:

        <SubstitutableText text="Hello <1>World</1>">
            {#snippet snippet1(text)}<a href="https://example.com/" target="_blank">{text}</a>{/snippet}
        </SubstitutableText>

    Licensing: This component is originally based on `src/app/ui/SubstitutableText.svelte` as part
    of Threema Desktop (https://github.com/threema-ch/threema-desktop/), which is released under the
    AGPLv3 license.
-->
<script lang="ts">
  import type {Snippet} from 'svelte';

  import {assertUnreachable, unreachable, unwrap} from '$lib/assert';

  interface Props {
    text: string | undefined;
    snippet1?: Snippet<[string]>;
    snippet2?: Snippet<[string]>;
    snippet3?: Snippet<[string]>;
  }

  let {text, snippet1, snippet2, snippet3}: Props = $props();

  // For now there are no instances of needing more than 3 different tags in a text. We can add more
  // if needed.
  const ALLOWED_TAGS = ['1', '2', '3'] as const;
  type AllowedTag = (typeof ALLOWED_TAGS)[number];

  function isAllowedTag(tag: string | undefined): tag is AllowedTag {
    if (tag === undefined) {
      return false;
    }
    return (ALLOWED_TAGS as readonly string[]).includes(tag);
  }

  function snippetForTag(tag: AllowedTag): Snippet<[string]> | undefined {
    switch (tag) {
      case '1':
        return snippet1;
      case '2':
        return snippet2;
      case '3':
        return snippet3;
      default:
        return unreachable(tag);
    }
  }

  const ALLOWED_TAGS_CHAR_SET = `[${ALLOWED_TAGS.join('')}]`;
  const SELF_CLOSING_TAG_PATTERN = `<(?<selfClosingTag>${ALLOWED_TAGS_CHAR_SET}) ?/>` as const;
  const TAG_PATTERN = `<(?<tag>${ALLOWED_TAGS_CHAR_SET})>(?<text>.*?)</\\k<tag>>` as const;
  const PLAIN_TEXT_PATTERN = `(?<plain>(?:.+?(?=<${ALLOWED_TAGS_CHAR_SET}(?: ?/)?>)|.+$))` as const;

  const TAG_SPLITTER_REGEX = new RegExp(
    [SELF_CLOSING_TAG_PATTERN, TAG_PATTERN, PLAIN_TEXT_PATTERN].join('|'),
    'gum',
  );

  type Fragment =
    | {readonly type: 'plain'; readonly text: string}
    | {readonly type: 'tag'; readonly tag: AllowedTag; readonly text: string}
    | {readonly type: 'selfClosingTag'; readonly tag: AllowedTag; readonly text: undefined};

  function warnMissingSlot(tag: AllowedTag): void {
    console.warn(
      `Text "${text}" expects a child snippet named \`snippet${tag}\` but it has not been provided.`,
    );
  }

  function warnUnusedSlots(newFragments: Fragment[]): void {
    const expectedTags = new Set(
      newFragments
        .map((fragment) => (fragment.type === 'plain' ? '' : fragment.tag))
        .filter(isAllowedTag),
    );
    for (const tag of ALLOWED_TAGS) {
      if (snippetForTag(tag) !== undefined && !expectedTags.has(tag)) {
        console.warn(`Unused child snippet named \`snippet${tag}\` for text "${text}".`);
      }
    }
  }

  const fragments = $derived(
    text === undefined
      ? []
      : [...text.matchAll(TAG_SPLITTER_REGEX)].map<Fragment>((match) => {
          const matchedText = match[0];
          const {groups} = match;

          if (groups === undefined) {
            return assertUnreachable('TAG_SPLITTER_REGEX should have returned a matched group');
          }

          if (groups.plain !== undefined) {
            return {
              type: 'plain',
              text: groups.plain,
            };
          }

          if (isAllowedTag(groups.tag)) {
            const tagText = unwrap(groups.text);
            if (snippetForTag(groups.tag) !== undefined) {
              return {
                type: 'tag',
                tag: groups.tag,
                text: tagText,
              };
            }
            warnMissingSlot(groups.tag);
            return {
              type: 'plain',
              text: import.meta.env.DEBUG ? matchedText : tagText,
            };
          }

          if (isAllowedTag(groups.selfClosingTag)) {
            if (snippetForTag(groups.selfClosingTag) !== undefined) {
              return {
                type: 'selfClosingTag',
                tag: groups.selfClosingTag,
              };
            }
            warnMissingSlot(groups.selfClosingTag);
            return {
              type: 'plain',
              text: import.meta.env.DEBUG ? matchedText : '',
            };
          }

          return assertUnreachable(`Unexpected matching by TAG_SPLITTER_REGEX on '${matchedText}'`);
        }),
  );

  $effect(() => {
    warnUnusedSlots(fragments);
  });
</script>

{#each fragments as fragment (fragment)}
  {#if fragment.type === 'plain'}
    {fragment.text}
  {:else if fragment.type === 'tag' || fragment.type === 'selfClosingTag'}
    {#if fragment.tag === '1'}
      {@render snippet1?.(fragment.text ?? '')}
    {:else if fragment.tag === '2'}
      {@render snippet2?.(fragment.text ?? '')}
    {:else if fragment.tag === '3'}
      {@render snippet3?.(fragment.text ?? '')}
    {:else}
      {unreachable(fragment.tag)}
    {/if}
  {:else}
    {unreachable(fragment)}
  {/if}
{/each}
