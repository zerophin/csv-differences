<script>
  import {data1, data2} from "./mock-csv";
  import Table from "./components/Table.svelte";
  import {SvelteComponent} from "svelte";

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
<h1>CSV Differences Bitches</h1>
<h2>
  CSV 1
</h2>
<textarea bind:value={csv1}></textarea>
<h2>
  CSV 2
</h2>
<textarea bind:value={csv2}></textarea>
<br/>
<Table diffs={diffs}/>
<style>

    textarea {
        height: 10em;
        width: 50%;
    }

</style>
