<script>
    import { onMount } from 'svelte'
    import wordList from "./assets/data.json"
    $: puzzleInput = ""
    $: puzzleInputWords = puzzleInput.split(" ")
    $: solutions = []
    $: solutionGenerated = false

    let puzzleInputBox
    onMount(() => puzzleInputBox.focus())

    const handleClick = (event) => {
      // Set focus on invisable input box when clicking anywhere on page
      puzzleInputBox.focus()
      
    }

    const findSolution = (inputWords) => {
      solutionGenerated = false
      solutions = []
      for (let i = 0; i < inputWords.length; i++) {
        let filter = new RegExp(inputWords[i].toLowerCase().replace(/\?/g, "."))
        let wordMatches = []
        for (const word in wordList) {
          if (filter.test(word) && inputWords[i].length == word.length) {
            var obj = {};
            obj[word] = wordList[word]
            wordMatches.push(obj)
          }
        }
        solutions.push(wordMatches)
      }
      solutionGenerated = true
      
    }

    
</script>

<!-- svelte-ignore a11y-click-events-have-key-events -->
<main on:click={handleClick}>

  <nav class="d-flex justify-content-center bg-dark text-white align-items-center">
    <h3>BILT Puzzle Solver</h3>
  </nav>

  <content class="container">
    <div class="row text-center">
      <div class="col fs-5 fw-bold">Type the puzzle input below. Use "?" for unknown characters.</div>
    </div>
    <div class="row text-center">
      <div class="col fs-6 fw-semibold">A table will be generated for each word of potential solutions</div>
    </div>

    <div class="row" style="width: 0; overflow: hidden">
      <div class="col">
        <input class="" type="text" name="userinput" id="userinput" bind:value={puzzleInput} bind:this={puzzleInputBox}>
      </div>
    </div>

      <div class="d-flex gap-3 justify-content-center flex-wrap">
        {#each puzzleInputWords as word}
          <div class="d-flex">
            {#if puzzleInput == ""}
            <div class="border-bottom m-1 border-dark border-1 fs-2 text-white">A</div>
            {/if}
            {#each word as char}
            <div class="border-bottom m-1 border-darkcat border-1 fs-2">{char.toUpperCase()}</div>
            {/each}
          </div>
        {/each}
      </div>
      <div class="d-flex justify-content-center mt-3">
        <button class="btn btn-success d-flex w-50 text-center justify-content-center" on:click={() => findSolution(puzzleInputWords)}>Generate Solutions</button>
      </div>

      {#if solutionGenerated}
      <h4 class="text-center mt-3">Potential Solutions:</h4>
      <div class="d-flex gap-3 mt-3 flex-wrap justify-content-center">
      {#each solutions as solution, count}
        <div>
          <div class="text-center">Word {count+1}</div>
          <div class="table-responsive w-100" style="max-height: 500px;">
            <table class="table table-sm text-center table-bordered">
              <thead>
                <tr>
                  <th>Word</th>
                  <th>Frequency</th>
                </tr>
              </thead>
              <tbody>
                {#each solution as item}
                  <tr>
                    <td>{Object.keys(item)[0]}</td>
                    <td>{Object.values(item)[0]}</td>
                  </tr>
                {/each}
                </tbody>
            </table>
          </div>
        </div>
      {/each}
      </div>
      {/if}
      

  </content>
</main>

<style>
  main {
    overflow: hidden;
  }
</style>

