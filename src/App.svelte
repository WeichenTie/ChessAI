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
  
  const pieces = {
    1 : {title: "white pawn", image: "/w_pawn.svg"},
    2 : {title: "white knight", image: "/w_knight.svg"},
    3 : {title: "white bishop", image: "/w_bishop.svg"},
    4 : {title: "white rook", image: "/w_rook.svg"},
    5 : {title: "white queen", image: "/w_queen.svg"},
    6 : {title: "white king", image: "/w_king.svg"},
    7 : {title: "black pawn", image: "/b_pawn.svg" },
    8 : {title: "black knight", image: "/b_knight.svg"},
    9 : {title: "black bishop", image: "/b_bishop.svg"},
    10 : {title: "black rook", image: "/b_rook.svg" },
    11 : {title: "black queen", image: "/b_queen.svg"},
    12 : {title: "black king", image: "/b_king.svg"},
  }
  
  function initFromFenString(fen: string) {
    // Get the values of the first 8 ranks
    const ranks = fen.split(' ')[0].split('/');
    let rankIndex = 0
    for (const rank of ranks) {
      let fileIndex = 0;
      for (const piece of rank) {
        switch (piece) {
          case 'P':
            board[rankIndex][fileIndex] = 1;
            break;
          case 'N':
            board[rankIndex][fileIndex] = 2;
            break;
          case 'B':
            board[rankIndex][fileIndex] = 3;
            break;
          case 'R':
            board[rankIndex][fileIndex] = 4;
            break;
          case 'Q':
            board[rankIndex][fileIndex] = 5;
            break;
          case 'K':
            board[rankIndex][fileIndex] = 6;
            break;
          case 'p':
            board[rankIndex][fileIndex] = 7;
            break;
          case 'n':
            board[rankIndex][fileIndex] = 8;
            break;
          case 'b':
            board[rankIndex][fileIndex] = 9;
            break;
          case 'r':
            board[rankIndex][fileIndex] = 10;
            break;
          case 'q':
            board[rankIndex][fileIndex] = 11;
            break;
          case 'k':
            board[rankIndex][fileIndex] = 12;
            break;
        }
        if (Number.isInteger(piece)) {
          fileIndex += parseInt(piece);
        } else {
          fileIndex++;
        }
      }
      rankIndex++;
    }
  }
  
  
  let selected = null;
  function onMouseDown(e) {
    e.preventDefault();
    // console.log(e.target);
  }
  
  function onMouseUp(e) {
    e.preventDefault();
  }
  
  initFromFenString('rnbqkbnr/pppppppp/8/8/8/8/PPPPPPPP/RNBQKBNR w KQkq - 0 1')
</script>

<main>
  <div class="chess-board">
    {#each board as row, ri}
      {#each row as cell, ci}
        <div class="cell" data-row-even={!(ri % 2)} data-col-even={!!(ci % 2)} data-has-piece={cell !== 0} on:mousedown={onMouseDown} on:mouseup{onMouseUp}>
          {#if cell !== 0}
            <object class={`piece cell-${ri}${ci}`} title={pieces[cell].title} data={pieces[cell].image} type="image/svg+xml" />
          {/if}
        </div>
      {/each}
    {/each}
  </div>
</main>

<style lang="postcss">
  @tailwind components;
  @layer components {
    .chess-board {
      @apply grid grid-cols-[repeat(8,1fr)] grid-rows-[repeat(8,1fr)] w-[512px] h-[512px];
    }
    .cell {
      @apply bg-slate-400 flex justify-center items-center;
    }
    .cell[data-row-even='true'][data-col-even='true'] {
      @apply bg-slate-600;
    }
    .cell[data-row-even='true'][data-col-even='false'] {
      /* @apply bg-slate-200; */
    }
    .cell[data-row-even='false'][data-col-even='true'] {
      /* @apply bg-slate-200; */
    }
    .cell[data-row-even='false'][data-col-even='false'] {
      @apply bg-slate-600;
    }
    .piece {
      @apply w-full aspect-square flex items-center justify-center p-1 pointer-events-none;
    }
    .cell[data-has-piece="true"] {
      @apply cursor-grab;
    }
  }
</style>
