<script>
  import XLSX from 'xlsx';

  export let inputName = ""
  export let csv = ''
  let file = '';

  function handleFile(e) {
    let files = e.target.files, f = files[0];
    let reader = new FileReader();
    reader.onload = function (e) {
      let workbook = XLSX.read(e.target.result);
      let sheetName = workbook.SheetNames[0];
      csv = XLSX.utils.sheet_to_csv(workbook.Sheets[sheetName]);
    };
    reader.readAsArrayBuffer(f);
  }
</script>

<div class="textarea-group">
    <h2>
        {inputName}
    </h2>
    <input type="file" accept=".xlsx, .xls, .csv" on:change={handleFile}/>
    <textarea bind:value={csv}></textarea>
</div>

<style>
    .textarea-group {
        display: flex;
        flex-direction: column;
        width: 100%;
    }

    textarea {
        height: 10em;
    }
</style>
