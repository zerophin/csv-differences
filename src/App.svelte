<script>
  import {data1, data2} from "./mock-csv";

  let diffs = [];
  let csv1 = data1
  let csv2 = data2
  $: csv1Arr = csvToObject(csv1)
  $: csv2Arr = csvToObject(csv2)

  function findErrors(arr1, arr2, keyTerm = 'id') {
    let errors = [];
    if (!Array.isArray(arr1) || !Array.isArray(arr2)) return [];
    arr1.forEach(person => {
      let personInColumn2 = arr2.find(two => person[keyTerm] === two[keyTerm])
      if (!personInColumn2) {
        return;
      }
      let noErrors = Object.keys(person).every(personKey => {
        return person[personKey] === personInColumn2[personKey];
      })
      if (!noErrors) {
        errors.push([person, personInColumn2]);
      }
    })
    return errors;
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
  Difference {i + 1}:
  <hr/>
  <h2>
    first
  </h2>
  <p>
    ID: {diff[0].ID} <br/>
    name: {diff[0].name}<br/>
    age: {diff[0].age}<br/>
  </p>
  <h2>
    second
  </h2>
  <p>
    ID: {diff[1].ID} <br/>
    name: {diff[1].name}<br/>
    age: {diff[1].age}<br/>
  </p>
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
        background: red;
    }
</style>
