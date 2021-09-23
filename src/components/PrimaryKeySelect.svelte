<script>
  export let primaryKey = ''
  export let primaryKeyOptions = [];
</script>
<label>
  Primary Key
  <select bind:value={primaryKey}>
    {#each primaryKeyOptions as key}
      <option value={key}>
        {key}
      </option>
    {/each}
  </select>
</label>
<small>Primary Key should be unique in both tables</small>
