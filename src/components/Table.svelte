<script>
  export let diffs = [];
</script>
<div class="tables">
  {#each diffs as diff, i}
    <div class="table-group">
      {#each Object.entries(diff).filter(([key]) => key !== "errorKeys") as [csvInput, csvValues]}
        <table>
          <caption>{csvInput}</caption>
          {#each Object.entries(csvValues) as [key, value]}
            <tr>
              <td><strong>{key}</strong></td>
              <td class:diffError={diff.errorKeys.includes(key)}>{value}</td>
            </tr>
          {/each}
          <!--          <tr>-->
          <!--            <td>Errors</td>-->
          <!--            <td>{diff.errorKeys}</td>-->
          <!--          </tr>-->
        </table>
      {/each}
    </div>
  {/each}
  {#if !(diffs.length > 0)}
    <p>
      There are no errors!
    </p>
  {/if}
</div>
<style>
    .tables {
        display: flex;
        flex-wrap: wrap;
        justify-content: left;
    }

    .table-group {
        display: flex;
        flex-direction: row;
        justify-content: space-between;
        margin-right: 2rem;
        width: 25%;
        min-width: 200px;
    }

    .table-group:first-child {
    }

    caption {
        font-size: 1.5rem;
        font-weight: bold;
    }

    table {
        width: 100%;
        border: 1px solid #000;
        display: block;
    }

    th, td {
        width: 20%;
        text-align: left;
        vertical-align: top;
        border: 1px solid #000;
        border-spacing: 0;
    }

    .diffError {
        background: red;
        color: white;
    }

    @media (max-width: 800px) {
        .tables {
            justify-content: center;
        }

        .table-group {
            width: 100%;
        }
    }

</style>
