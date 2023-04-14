<script>
  let countdown = 25 * 60
  let timer
  let running = false
  let cycles = 0
  let currentState = "work"

  function toggleTimer() {
    if (running) {
      clearInterval(timer)
      running = false
    } else {
      timer = setInterval(() => {
        countdown -= 1
        if (countdown === 0) {
          clearInterval(timer)
          running = false
          cycles += 1
          if (cycles % 4 === 0) {
            currentState = "long break"
            countdown = 15 * 60
          } else {
            currentState = "break"
            countdown = 5 * 60
          }
          toggleTimer()
        }
      }, 1000)
      running = true
    }
  }
  let time
  $: {
    const [min, sec] = [Math.floor(countdown / 60), countdown % 60]
    time = `${min < 10 ? "0" : ""}${min}:${sec < 10 ? "0" : ""}${sec}`
  }
</script>

<div class="flex justify-center items-center">
  <div
    class="flex justify-center items-center flex-col space-y-10 rounded-3xl p-10 m-10 bg-slate-900">
    <div class="flex space-x-5 bg-slate-800 p-2 rounded-full">
      <button
        class={`text-xl px-5 py-2 rounded-full duration-150 font-mono ${
          currentState === "work" ? "bg-slate-700" : "bg-transparent"
        }`}
        on:click={() => {
          currentState = "work"
          countdown = 25 * 60
          toggleTimer()
        }}>
        Work
      </button>
      <button
        class={`text-xl px-5 py-2 rounded-full duration-150 font-mono ${
          currentState === "break" ? "bg-slate-700" : "bg-transparent"
        }`}
        on:click={() => {
          currentState = "break"
          countdown = 5 * 60
          toggleTimer()
        }}>
        Break
      </button>
      <button
        class={`text-xl px-5 py-2 rounded-full duration-150 font-mono ${
          currentState === "long break" ? "bg-slate-700" : "bg-transparent"
        }`}
        on:click={() => {
          currentState = "long break"
          countdown = 15 * 60
          toggleTimer()
        }}>
        Long Break
      </button>
    </div>

    <div class="text-9xl text-center">
      {time}
    </div>

    <button
      on:click={toggleTimer}
      class={`text-5xl px-10 py-5 rounded-full duration-150 font-mono ${
        running
          ? "bg-rose-800 hover:bg-rose-700"
          : "bg-emerald-700 hover:bg-emerald-600"
      }`}>
      {running ? "PAUSE" : "START"}
    </button>
    <button
      on:click={() => {
        cycles = 0
        clearInterval(timer)
        running = false
        currentState = "work"
        countdown = 25 * 60
      }}
      class="text-5xl px-10 py-5 rounded-full duration-150 bg-slate-700 hover:bg-slate-600 active:bg-slate-800 font-mono">
      RESET
    </button>
    <div class="text-3xl text-slate-500">Number of cycles: {cycles}</div>
  </div>
</div>
