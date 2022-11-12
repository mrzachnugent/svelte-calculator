<script lang="ts">
const options = ["AC", "±", "%", "÷", 7, 8, 9, "⨉", 4, 5, 6, "-", 1, 2, 3, "+", 0, ".", "="]
let display = "0"
let result = 0
let operation = ""
let reset = true

const isMisc = (option: string | number) => {
    switch (option) {
        case "AC":
        case "±":
        case "%":
            return true
        default:
            return false
    }
}
const isOperation = (option: string | number) => {
    switch (option) {
        case "÷":
        case "⨉":
        case "-":
        case "+":
        case "=":
            return true
        default:
            return false
    }
}

function handleOnClick(e) {
    console.log({
        result,
        operation,
        display,
    })
    if (isOperation(e.target.innerText)) {
        const displayInt = parseInt(display)
        switch (operation) {
            case "÷":
                result = result !== 0 ? result / displayInt : displayInt
                operation = e.target.innerText === "=" ? "" : e.target.innerText
                display = String(result)
                break;
            case "⨉":
                result = result === 0 ? displayInt : result * displayInt
                operation = e.target.innerText === "=" ? "" : e.target.innerText
                display = result ? String(result) : display
                break
            case "-":
                result = result !== 0 ? result - displayInt : displayInt
                operation = e.target.innerText === "=" ? "" : e.target.innerText
                display = String(result)
                break
            case "+":
                result = result !== 0 ? result + displayInt : displayInt
                operation = e.target.innerText === "=" ? "" : e.target.innerText
                display = String(result)
                break;
        }
    }
    switch (e.target.innerText) {
        case "AC":
            display = "0"
            result = 0
            operation = ""
            break;
        case "±":
            display = String(parseInt(display) * -1)
            break;
        case "%":
            display = display.includes(".") ? display : String(parseInt(display) * 0.01)
            break;
        case "÷":
        case "⨉":
        case "-":
        case "+":
            result = result === 0 ? parseInt(display) : result
            operation = e.target.innerText
            reset = true
            break
        case "=":
            display = result ? String(result) : display
            reset = true
            break
        case ".":
            display = display.includes(".") || display.length === 7 ? display : display + "."
            break
        default:
            display = display.length >= 7 ? display : display === "0" || reset ? e.target.innerText : display + e.target.innerText
            reset = false
    }
}
</script>

<main class="min-h-screen flex justify-center items-center">
    <div class="bg-gray-600 w-80 h-[40rem] p-px rounded-[2rem] shadow-2xl">
        <div class="bg-black h-full w-full max-w-xs rounded-[2rem] flex flex-col justify-between px-4">
            <div class="text-gray-100 text-xs flex justify-between h-14 px-4 items-center">
                <div>4:20</div>
                <div class="flex items-center gap-2"><span>100%</span>
                    <div class="h-2 w-4 bg-white border-black border-2 rounded-sm ring-2 ring-gray-700 ring-offset-1"></div>
                </div>
            </div>
            <div class="flex-1 flex justify-end items-end p-1 mb-3 overflow-hidden">
                <p class="text-white"><span class="{display.length < 7 ? "text-7xl" : "text-6xl" }">{display}</span></p>
            </div>
            <div class="flex-[2] grid grid-cols-4 grid-rows-5 gap-2 grid-flow-row-dense pb-1">

                {#each options as option}
                <button on:click={handleOnClick} class="{isMisc(option) ? "bg-gray-400 rounded-full transition-all active:bg-gray-500" : isOperation(option) ? "bg-orange-400 transition-all active:bg-orange-500 text-white rounded-full" : option === 0 ? "bg-gray-700 transition-all active:bg-gray-800 text-white col-span-2 rounded-full" : "bg-gray-700 transition-all active:bg-gray-800 text-white rounded-full"}">
                    <div  class="h-16 w-16 text-2xl rounded-full flex justify-center items-center">{option}</div>
                </button>
                {/each}
            </div>

            <div class="h-10 flex justify-center items-center">
                <div class="h-[3px] w-16 bg-gray-300"></div>
            </div>
        </div>
    </div>
</main>
