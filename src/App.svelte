<script>
  let rows = Array(9).fill(Array(9).fill(''));

  let cellElements = {};

  const cellId = (rowIndex, cellIndex) => `${rowIndex}-${cellIndex}`;

  const onKeyDown = (event, rowIndex, cellIndex) => {
    if ('123456789'.includes(event.key)) {
      rows[rowIndex] = rows[rowIndex].map((cell, ci) => ci === cellIndex ? event.key : cell) 
    }

    if (event.key === 'ArrowLeft' && cellIndex > 0) {
      cellElements[cellId(rowIndex, cellIndex - 1)].focus();
    }

    if (event.key === 'ArrowUp' && rowIndex > 0) {
      cellElements[cellId(rowIndex - 1, cellIndex)].focus();
    }

    if (event.key === 'ArrowRight' && cellIndex < 8) {
      cellElements[cellId(rowIndex, cellIndex + 1)].focus();
    }

    if (event.key === 'ArrowDown' && rowIndex < 8) {
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
          on:keydown|preventDefault={e => onKeyDown(e, rowIndex, cellIndex)}
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
