<script>
  let rows = Array(9).fill(Array(9).fill(''));

  const getRowClassName = rowIndex =>
    (rowIndex + 1) % 3 === 0 ? 'border-bottom' : '';

  const getCellClassName = cellIndex =>
    (cellIndex + 1) % 3 === 0 ? 'border-right' : '';

  const onChange = (value, rowIndex, cellIndex, e) => {
    console.log('called')

    if (value.length <= 1 && '123456789'.includes(value)) {
      rows[rowIndex] = rows[rowIndex].map((cell, ci) => ci === cellIndex ? value : cell)
    }

    e.currentTarget.value = rows[rowIndex][cellIndex]; // weird svelte hack to keep control
  }

  $: console.log(rows)

</script>

<main>
  <div class='grid'>
    {#each rows as row, rowIndex (rowIndex)}
      <div class={getRowClassName(rowIndex)}>
      {#each row as cell, cellIndex}
        <input
          type='text'
          class={`cell ${getCellClassName(cellIndex)}`}
          value={cell}
          on:input={e => onChange(e.currentTarget.value, rowIndex, cellIndex, e)}
        />
      {/each}
      </div>
    {/each}
  </div>
</main>

<style>
  main{
    text-align: center;
  }

  .grid {
    display: inline-block;
    border: 2px solid black;
  }

  .cell {
    display: inline-block;
    height: 2rem;
    width: 2rem;
    border: 1px solid black;
    margin: 0;
    text-align: center;
  }

  .border-bottom {
    border-bottom: 2px solid black;
  }

  .border-right {
    border-right: 2px solid black;
  }
</style>
