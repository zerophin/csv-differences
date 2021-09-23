<script>
  import {data1, data2} from "./mock-csv";
  import Table from "./components/Table.svelte";
  import PrimaryKeySelect from "./components/PrimaryKeySelect.svelte";

  let diffs = [];
  let csv1 = data1
  let csv2 = data2
  let primaryKey = 'id'
  $: primaryKeyOptions = csv1Arr.length ? Object.keys(csv1Arr[0]) : [];
  $: csv1Arr = csvToObject(csv1)
  $: csv2Arr = csvToObject(csv2)

  function findErrors(arr1, arr2, keyTerm = 'id') {
    let rowDifferences = [];
    if (!Array.isArray(arr1) || !Array.isArray(arr2)) return [];
    arr1.forEach(row => {
      let row2 = arr2.find(two => row[keyTerm] === two[keyTerm])
      if (!row2) {
        return;
      }
      let errors = []
      Object.keys(row).forEach(key => {
        let noError = row[key] === row2[key];
        if (!noError) {
          errors.push(key)
        }
      })
      if (errors.length > 0) {
        let newErrObj = {
          csv1: row,
          csv2: row2,
          errorKeys: errors,
        }
        rowDifferences.push(newErrObj);
      }
    })
    return rowDifferences;
  }

  // returns [{column: row, column: row}]
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

  $:diffs = findErrors(csv1Arr, csv2Arr, primaryKey)


</script>
<h1>CSV Differences</h1>
<PrimaryKeySelect bind:primaryKey bind:primaryKeyOptions/>
<div class="csv-inputs">
  <div class="textarea-group">
    <h2>
      CSV 1
    </h2>
    <textarea bind:value={csv1}></textarea>
  </div>
  <div class="textarea-group">
    <h2>
      CSV 2
    </h2>
    <textarea bind:value={csv2}></textarea>
  </div>
</div>
<br/>
<Table diffs={diffs}/>
<style>
    .csv-inputs {
        display: flex;
    }

    .textarea-group {
        display: flex;
        flex-direction: column;
        width: 100%;
    }

    textarea {
        height: 10em;
    }

    @media (max-width: 800px) {
        .csv-inputs {
            width: 100%;
            flex-direction: column;
        }
    }

</style>
