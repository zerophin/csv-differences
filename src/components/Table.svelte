<script>
  export let diffs = [];
</script>
<div class="diff-collection">
  {#each diffs as diff, i}
    <div class="diff-row">
      {#each Object.entries(diff).filter(([key]) => key !== "errorKeys") as [csvInput, csvValues]}
        <div class="csv-row">
          <h2>{csvInput}</h2>
          {#each Object.entries(csvValues) as [key, value]}
            <p class:diffError={diff.errorKeys.includes(key)}>{key} - {value}</p>
          {/each}
        </div>
      {/each}
      <pre>Errors {diff.errorKeys}</pre>
    </div>
  {/each}
  {#if !(diffs.length > 0)}
    <p>
      There are no errors!
    </p>
  {/if}
</div>
<style>

    .diff-collection {
        display: flex;
    }

    .diff-row {
        display: flex;
        flex-direction: row;
        justify-content: center;
        width: 20%
    }

    .csv-row {
        padding: 2em;
    }

    .diffError {
        background: red;
        color: white;
        padding: .5rem;
    }
</style>
