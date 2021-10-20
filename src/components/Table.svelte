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
        display: grid;
        grid-template-columns: repeat(3, 1fr);
        gap: 1em;
        justify-content: space-around;
    }

    .table-group {
        display: flex;
        flex-direction: row;
        justify-content: space-between;
        margin-right: 2rem;
        margin-bottom: 2rem;
        min-width: 200px;
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

    tr {
        width: 100%;
    }

    th, td {
        width: 50%;
        text-align: left;
        vertical-align: top;
        border: 1px solid #000;
        border-spacing: 0;
    }

    .diffError {
        background: red;
        color: white;
    }

    @media (max-width: 1000px) {
        .tables {
            justify-content: center;
            grid-template-columns: 1fr;
        }

        .table-group {
            width: 100%;
        }
    }

</style>
