<script>
  import { getContext } from "svelte";
  import XeroPanel from "./XeroPanel.svelte";
  export let text;
  export let after_submit;
  export let before_submit;

  const { styleable } = getContext("sdk");
  const component = getContext("component");
  
  const beforeSubmit = (e) => {
        if(before_submit){
        before_submit({ before_submit_value: e.detail });
        }
    };

    const afterSubmit = (e) => {
        if(after_submit){
          after_submit({ after_submit_value: e.detail });
        }
    };
</script>

<div use:styleable={$component.styles}>
  <XeroPanel bind:text={text} on:after_submit={afterSubmit} on:before_submit={beforeSubmit}  />
</div>

