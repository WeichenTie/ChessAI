<script lang="ts">
  import bBishop from './assets/b_bishop.svg';
  import bKing from './assets/b_king.svg';
  import bKnight from './assets/b_knight.svg';
  import bPawn from './assets/b_pawn.svg';
  import bQueen from './assets/b_queen.svg';
  import bRook from './assets/b_rook.svg';
  
  import wBishop from './assets/w_bishop.svg';
  import wKing from './assets/w_king.svg';
  import wKnight from './assets/w_knight.svg';
  import wPawn from './assets/w_pawn.svg';
  import wQueen from './assets/w_queen.svg';
  import wRook from './assets/w_rook.svg';
  import { fade } from 'svelte/transition';
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
    1 : {title: "white pawn", image:bPawn },
    2 : {title: "white knight", image:bKnight },
    3 : {title: "white bishop", image:bBishop },
    4 : {title: "white rook", image:bRook },
    5 : {title: "white queen", image:bQueen },
    6 : {title: "white king", image:bKing },
    7 : {title: "black pawn", image:wPawn },
    8 : {title: "black knight", image:wKnight },
    9 : {title: "black bishop", image:wBishop },
    10 : {title: "black rook", image:wRook },
    11 : {title: "black queen", image:wQueen },
    12 : {title: "black king", image:wKing },
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
  
  const mouseCoords = { x: 0, y: 0 };
  let selected = null;
  function onMouseDown(e) {
    e.preventDefault();
    const piece = e.target.children[0];
    if (!piece) return;
    selected = {row: Number.parseInt(piece.dataset.row), col: Number.parseInt(piece.dataset.col)};
  }
  
  function onMouseMove(e) {
    mouseCoords.x = Number.parseInt(event.x);
    mouseCoords.y = Number.parseInt(event.y);
  }
  
  function onMouseUp(e) {
    e.preventDefault();
    if (!selected) return;
    const target = {row: e.target.dataset.row, col: e.target.dataset.col}
    board[target.row][target.col] = board[selected.row][selected.col]
    board[selected.row][selected.col] = 0;
    selected = null
  }
  
  initFromFenString('rnbqkbnr/pppppppp/8/8/8/8/PPPPPPPP/RNBQKBNR w KQkq - 0 1')
</script>

<svelte:window on:mouseup={()=>selected = null} />

<main>
  <div id="chess-board" on:mousemove={onMouseMove}  style:--mouse-x={`${mouseCoords.x}px`} style:--mouse-y={`${mouseCoords.y}px`}>
    {#each board as row, ri}
      {#each row as cell, ci}
        <div class="cell" on:mouseup={onMouseUp} style:--row={ri} style:--col={ci} data-row-even={!(ri % 2)} data-col-even={!!(ci % 2)} data-row={ri} data-col={ci} data-has-piece={cell !== 0} on:mousedown={onMouseDown} >
          {#if cell !== 0}
            <object class={`piece cell-${ri}${ci}`} title={pieces[cell].title} data={pieces[cell].image} data-row={ri} data-col={ci} type="image/svg+xml" data-selected={ri === selected?.row && ci === selected?.col}/>
          {/if}
        </div>
      {/each}
    {/each}
  </div>
</main>

<style lang="postcss">
  @tailwind components;
  @layer components {
    #chess-board {
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
    [data-selected="true"] {
      transform: translate(-50%, -50%) translate(calc(var(--col) * -100% + var(--mouse-x, 0px)), calc(var(--row) * -100% + var(--mouse-y, 0px)));
    }
  }
</style>
