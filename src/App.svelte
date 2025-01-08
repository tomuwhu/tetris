<script>
  import { onMount } from "svelte";
  onMount(() => {
    setInterval(() => {
      f()
    },1000)
  })
  var pálya = Array.from({ length: 21 }, _ => 
    Array.from({ length: 10 }, _ => 0)
  )
  pálya[20] = pálya[20].map(_ => 10)
  var alakzatok = [
    [[1, 0], [1, 0], [1, 1]],
    [[0, 2], [0, 2], [2, 2]],
    [[3, 3, 3], [0, 3, 0]],
    [[4, 4, 4, 4]],
    [[5, 5], [5, 5]]
  ]
  var gameover = false
  var score = 0
  var ap = [0, 4]
  var aa = alakzatok[[0,1,2,3,4].sort((a, b) => Math.random()-0.5)[0]]
  window.onkeydown = e => {
    if (gameover) return
    if (e.key == "ArrowLeft" && ap[1] > 0) {
      ap[1]--
    }
    if (e.key == "ArrowRight" && ap[1] + aa[0].length < 10) {
      ap[1]++
    }
    if (e.key == "ArrowDown") {
      f()
    }
    if (e.key == "ArrowUp") {
      var fa = Array.from({length: aa[0].length}, _ => [])
      aa.forEach((row, y) => {
        row.forEach((c, x) => fa[x][aa.length-y-1] = c)
      })
      aa = fa
    }
  }
  function f() {
    if (gameover) return
    let vége = false
    aa.forEach((row, y) => {
      row.forEach((cell, x) => {
        if (cell && pálya[ap[0] + y + 1][ap[1] + x]) vége = true
      })
    })   
    if (vége) {
      aa.forEach((row, y) => {
        row.forEach((cell, x) => {
          if (cell) pálya[ap[0] + y][ap[1] + x] = cell
        })
      })    
      ap = [0, 4]
      aa = alakzatok[[0,1,2,3,4].sort((a, b) => Math.random()-0.5)[0]]
      aa.forEach((row, y) => {
        row.forEach((cell, x) => {
          if (cell && pálya[ap[0] + y][ap[1] + x]) gameover = true
        })
      })    
    } else {
      if (!gameover) { 
        ap[0]++
        score++
      }
    }
  }
</script>

<main>
  <h2>Tetris</h2>
  <div class="pálya">
    {#each pálya as sor, i}
      {#each sor as oszlop, j}
        {#if i<20}
        {#if i >= ap[0] && i < ap[0] + aa.length && j>=ap[1] && j<ap[1] + aa[0].length}
          <div class="elem e{aa[i - ap[0]][j-ap[1]]} e{oszlop}"></div>
        {:else}
          <div class="elem e{oszlop}"></div>
        {/if}
        {/if}
      {/each}
    {/each}
  </div>
  <div>Score: {score}</div>
  {#if gameover}
  <div>Game Over</div>
  {/if}
</main>

<style>
  .pálya {
    display: grid;
    grid-template-columns: repeat(10, 1fr);
    grid-template-rows: repeat(20, 1fr);
  }
  .elem {
    border: 0.3px solid rgb(128, 49, 49);
    width: 20px;
    height: 20px;
    text-align: center;
  }
  .e0 {
    background-color: rgb(35, 6, 3);
  }
  .e1 {
    background-color: rgb(0, 229, 255);
  }
  .e2 {
    background-color: rgb(0, 229, 255);
  }
  .e3 {
    background-color: rgb(183, 255, 0);
  }
  .e4 {
    background-color: rgb(208, 0, 255);
  }
  .e5 {
    background-color: rgb(255, 115, 0);
  }
  .e6 {
    background-color: rgb(0, 8, 255);
  }
</style>
