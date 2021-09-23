<script>
  import {data1, data2} from "./mock-csv";

  let diffs = [];
  let csv1 = data1
  let csv2 = data2
  $: csv1Arr = csvToObject(csv1)
  $: csv2Arr = csvToObject(csv2)

  function findErrors(arr1, arr2, keyTerm = 'id') {
    let rowDifferences = [];
    if (!Array.isArray(arr1) || !Array.isArray(arr2)) return [];
    arr1.forEach(person => {
      let personInColumn2 = arr2.find(two => person[keyTerm] === two[keyTerm])
      if (!personInColumn2) {
        return;
      }
      let errors = []
      Object.keys(person).forEach(personKey => {
        let noError = person[personKey] === personInColumn2[personKey];
        if (!noError) {
          errors.push(personKey)
        }
      })
      //z
      // let noErrors = Object.keys(person).reduce((a, b, i) => {
      //   let isEqual = person[i] === personInColumn2[i];
      //   return isEqual ? a : [...a,]
      // }, [])
      if (errors.length > 0) {
        //rowDifferences.push([person, personInColumn2]);
        let newErrObj = {
          csv1: person,
          csv2: personInColumn2,
          errorKeys: errors,
        }
        rowDifferences.push(newErrObj);
      }
    })
    return rowDifferences;
  }

  function csvToObject(csv) {
    if (csv.length < 1) return;
    let csvArr = csv.trim().split('\n');
    let csvKeys = csvArr[0].split(',');
    let arr = [];
    for (let i = 1; i < csvArr.length; i++) {
      let newObj = {};
      let csvVals = csvArr[i].split(',');
      for (let j = 0; j < csvKeys.length; j++) {
        let key = csvKeys[j];
        let val = csvVals[j];
        newObj[key] = val;
      }
      arr.push(newObj)
    }
    return arr
  }

  $:diffs = findErrors(csv1Arr, csv2Arr, "ID")
</script>
<h1>
  CSV 1
</h1>
<textarea bind:value={csv1}></textarea>
<h1>
  CSV 2
</h1>
<textarea bind:value={csv2}></textarea>
<br/>
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
<style>
    textarea {
        height: 10em;
        width: 50%;
    }

    .diff-row {
        display: flex;
    }

    .csv-row {
        padding: 2em;
    }

    .diffError {
        color: red;
    }
</style>
