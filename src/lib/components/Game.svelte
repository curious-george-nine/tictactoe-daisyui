<script>
  import { fly } from "svelte/transition";

  /**
   * @type {string[]}
   */
  let board = ["", "", "", "", "", "", "", "", ""];

  let currentActivePlayer = "〇";

  let winner = "";

  /**
   * @type {HTMLDialogElement}
   */
  let winnerModal;

  let alreadyPlayed = false;

  let boardX = 0;

  /**
   * @param {number} i
   */
  function tileClick(i) {
    if (board[i] == "") {
      board[i] = currentActivePlayer;

      if (checkWon(currentActivePlayer)) {
        winner = currentActivePlayer;
        winnerModal.showModal();
        alreadyPlayed = true;
        return;
      }

      currentActivePlayer = currentActivePlayer == "〇" ? "×" : "〇";
    }
  }

  function playAgain() {
    board = ["", "", "", "", "", "", "", "", ""];

    currentActivePlayer = "〇";

    winner = "";

    boardX = window.screen.width * -1;

    setTimeout(() => {
      boardX = 0;
    }, 300);
  }

  /**
   * @param {string} p
   */
  function checkWon(p) {
    let hasWon = false;

    hasWon = board[0] == p && board[1] == p && board[2] == p;
    if (!hasWon) hasWon = board[3] == p && board[4] == p && board[5] == p;
    if (!hasWon) hasWon = board[6] == p && board[7] == p && board[8] == p;

    if (!hasWon) hasWon = board[0] == p && board[3] == p && board[6] == p;
    if (!hasWon) hasWon = board[1] == p && board[4] == p && board[7] == p;
    if (!hasWon) hasWon = board[2] == p && board[5] == p && board[8] == p;

    if (!hasWon) hasWon = board[1] == p && board[4] == p && board[8] == p;
    if (!hasWon) hasWon = board[2] == p && board[4] == p && board[6] == p;

    if (hasWon) return true;

    return false;
  }
</script>

<dialog bind:this={winnerModal} class="modal">
  <form method="dialog" class="modal-box">
    <h3 class="font-bold text-lg">Winner: {winner}</h3>
    <div class="my-4">
      <button class="btn btn-primary">Close</button>
      <button class="btn btn-accent" on:click={playAgain}>Play again</button>
    </div>
  </form>
  <form method="dialog" class="modal-backdrop">
    <button>close</button>
  </form>
</dialog>

<div
  class="grid grid-cols-3 w-[36rem] h-[36rem] gap-8 mx-auto my-12 transition"
  style="transform: translateX({boardX}px)"
>
  {#each board as tile, i}
    <button
      class="block bg-base-300 text-2xl aspect-square rounded-xl font-bold"
      on:click={() => tileClick(i)}>{tile}</button
    >
  {/each}
</div>

{#if alreadyPlayed}
  <button
    class="btn btn-accent my-12"
    on:click={playAgain}
    transition:fly={{ y: -20, duration: 400 }}>play again</button
  >
{/if}
