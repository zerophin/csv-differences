<script>
  import XLSX from 'xlsx';

  export let csv = ''
  let file = '';

  function handleFile(e) {
    let files = e.target.files, f = files[0];
    let reader = new FileReader();
    reader.onload = function (e) {
      let workbook = XLSX.read(e.target.result);
      let sheetName = workbook.SheetNames[0];
      console.log(workbook)
      csv = XLSX.utils.sheet_to_csv(workbook.Sheets[sheetName]);
      /* DO SOMETHING WITH workbook HERE */
    };
    reader.readAsArrayBuffer(f);
  }
</script>

<div class="textarea-group">
    <h2>
        CSV 1
    </h2>
    <input type="file" on:change={handleFile}/>
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
