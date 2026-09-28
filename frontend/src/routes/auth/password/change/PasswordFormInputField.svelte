<script lang="ts">
  interface Props {
    id: string;
    label: string;
    required?: boolean;
    error?: boolean;
    icon: string;
    value?: string;
  }

  let {id, label, required = false, error = false, icon, value = $bindable('')}: Props = $props();

  // TODO: Should this be used for other forms as well?
</script>

<div class="field">
  <label class="label" for={id}>{label}</label>
  <div class="control has-icons-left">
    <input {id} class="input" class:error type="password" bind:value {required} />
    <span class="icon is-small is-left">
      <i class="fa-solid {icon}"></i>
    </span>
  </div>
</div>

<style>
  .field input.error {
    border: 1px solid #ff3860;
  }
</style>
