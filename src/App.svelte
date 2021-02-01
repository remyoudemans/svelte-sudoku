<script>
  let rows = Array(9).fill(Array(9).fill(''));

  let cellElements = {};

  const onChange = (value, rowIndex, cellIndex, e) => {
    if (value.length <= 1 && '123456789'.includes(value)) {
      // weird cause svelte sucks at nested arrays
      rows[rowIndex] = rows[rowIndex].map((cell, ci) => ci === cellIndex ? value : cell) 
    }

    e.currentTarget.value = rows[rowIndex][cellIndex]; // weird svelte hack to keep control
  }

  /* $: console.log(rows) */
  /* $: console.log('cellElements:', cellElements) */

  const cellId = (rowIndex, cellIndex) => `${rowIndex}-${cellIndex}`;

  const moveCursor = (keyCode, rowIndex, cellIndex) => {
    if (keyCode === 37 && cellIndex > 0) {
      cellElements[cellId(rowIndex, cellIndex - 1)].focus();
    }

    if (keyCode === 38 && rowIndex > 0) {
      cellElements[cellId(rowIndex - 1, cellIndex)].focus();
    }

    if (keyCode === 39 && cellIndex < 8) {
      cellElements[cellId(rowIndex, cellIndex + 1)].focus();
    }

    if (keyCode === 40 && rowIndex < 8) {
      cellElements[cellId(rowIndex + 1, cellIndex)].focus();
    }

  }

</script>

<main>
  <div class='grid'>
    {#each rows as row, rowIndex}
      <div class:border-bottom={(rowIndex + 1) % 3 === 0}>
      {#each row as cell, cellIndex}
        <input
          bind:this={cellElements[cellId(rowIndex, cellIndex)]}
          type='text'
					class='cell'
					class:border-right={(cellIndex + 1) % 3 === 0}
          value={cell}
          on:keydown={e => moveCursor(e.keyCode, rowIndex, cellIndex)}
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
