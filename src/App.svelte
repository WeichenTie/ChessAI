<script lang="ts">
  import './app.css';
  const board = [
    [0, 0, 0, 0, 0, 0, 0, 0],
    [0, 0, 0, 0, 0, 0, 0, 0],
    [0, 0, 0, 0, 0, 0, 0, 0],
    [0, 0, 0, 0, 0, 0, 0, 0],
    [0, 0, 0, 0, 0, 0, 0, 0],
    [0, 0, 0, 0, 0, 0, 0, 0],
    [0, 0, 0, 0, 0, 0, 0, 0],
    [0, 0, 0, 0, 0, 0, 0, 0],
  ];
</script>

<main>
  <div class="chess-board">
    {#each board as row, ri}
      {#each row as cell, ci}
        <div class="cell" data-row-even={!(ri % 2)} data-col-even={!!(ci % 2)} />
      {/each}
    {/each}
  </div>
</main>
<object title="white king" data="/w_king.svg" type="image/svg+xml" />
<object title="white knight" data="/w_knight.svg" type="image/svg+xml" />
<object title="white bishop" data="/w_bishop.svg" type="image/svg+xml" />
<object title="white queen" data="/w_queen.svg" type="image/svg+xml" />
<object title="white rook" data="/w_rook.svg" type="image/svg+xml" />
<object title="white pawn" data="/w_pawn.svg" type="image/svg+xml" />
<object title="black king" data="/b_king.svg" type="image/svg+xml" />
<object title="black knight" data="/b_knight.svg" type="image/svg+xml" />
<object title="black bishop" data="/b_bishop.svg" type="image/svg+xml" />
<object title="black queen" data="/b_queen.svg" type="image/svg+xml" />
<object title="black rook" data="/b_rook.svg" type="image/svg+xml" />
<object title="black pawn" data="/b_pawn.svg" type="image/svg+xml" />

<style lang="postcss">
  @tailwind components;
  @layer components {
    .chess-board {
      @apply grid grid-cols-[repeat(8,1fr)] grid-rows-[repeat(8,1fr)] w-[512px] h-[512px];
    }
    .cell {
      @apply bg-slate-800;
    }
    .cell[data-row-even='true'][data-col-even='true'] {
      @apply bg-slate-900;
    }
    .cell[data-row-even='true'][data-col-even='false'] {
      /* @apply bg-slate-200; */
    }
    .cell[data-row-even='false'][data-col-even='true'] {
      /* @apply bg-slate-200; */
    }
    .cell[data-row-even='false'][data-col-even='false'] {
      @apply bg-slate-900;
    }
  }
</style>
