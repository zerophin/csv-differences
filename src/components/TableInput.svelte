<script>
  import XLSX from 'xlsx';

  export let inputName = ""
  export let csv = ''
  let file = '';
  let fileName = '';

  function handleFile(e) {
    let files = e.target.files, f = files[0];
    let reader = new FileReader();
    reader.onload = function (e) {
      let workbook = XLSX.read(e.target.result);
      let sheetName = workbook.SheetNames[0];
      csv = XLSX.utils.sheet_to_csv(workbook.Sheets[sheetName]);
    };
    reader.readAsArrayBuffer(f);
    if (f) {
      fileName = f.name;
    }
  }
</script>

<div class="textarea-group">
    <label for={`file-${inputName}`}>
        {inputName} {fileName.length > 0 ? `(${fileName})` : ""}
    </label>
    <input type="file" accept=".xlsx, .xls, .csv" id={`file-${inputName}`} on:change={handleFile}/>
    <textarea bind:value={csv}></textarea>
</div>

<style>

    label {
        font-size: 1.2rem;
        font-weight: bold;
        margin: 1rem 0;
    }

    .textarea-group {
        display: flex;
        flex-direction: column;
        width: 100%;
        margin-top: 2em;
    }

    textarea {
        height: 10em;
    }
</style>
