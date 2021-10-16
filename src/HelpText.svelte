<script>
  import Text from "./Text.svelte";

  export let x;
  export let y;
</script>

<Text
  text={`press space to pause`}
  fontSize="12"
  fontFamily="Courier New"
  align="left"
  baseline="top"
  {x}
  {y}
/>

<!-- The following allows this component to nest children -->
<slot />
