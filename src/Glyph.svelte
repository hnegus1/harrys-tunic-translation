<script>
    import { createEventDispatcher } from 'svelte';

    const dispatch = createEventDispatcher();

    export let glyphJson;
    export let languageJson;

    let swapSounds = false;

    let glpyhsParts = glyphJson.default.map(x => {
        return {
            ...x,
            checked: false
        }
    });
    let language = [...languageJson.default];

    export let currentSound;

    console.log(glpyhsParts)
    
    function handleClick(ev){
        //console.log(ev.target.id);

        const part = glpyhsParts.filter(x => x.id == ev.target.id)[0];
        const i = glpyhsParts.indexOf(part);
        glpyhsParts[i].checked = !glpyhsParts[i].checked

        evaluateWord()
    }

    function handleDelete(ev){
        dispatch('delete');
    }

    function toggleSwapSounds(){
        swapSounds = !swapSounds;
        evaluateWord()
    }

    function evaluateWord(){
        const checkedGlyphParts = glpyhsParts.filter(x => x.checked).map(x => parseInt(x.id.split("_")[1]))

        const checkVowelParts = glpyhsParts.filter(x => x.checked && x.isVowel).map(x => parseInt(x.id.split("_")[1]))
        const checkConsonantParts = glpyhsParts.filter(x => x.checked && !x.isVowel).map(x => parseInt(x.id.split("_")[1]))
        //console.log(checkedGlyphParts)
        
        const glyphSearch = language.filter(x => areEqual(x.conditions, checkedGlyphParts));

        const vowelSearch = language.filter(x => areEqual(x.conditions, checkVowelParts));
        const consonantSearch = language.filter(x => areEqual(x.conditions, checkConsonantParts));
        //console.log(language)
        //console.log(glyphSearch)

        let vowelSound = "";
        let consonantSound = ""

        if(vowelSearch.length === 1) vowelSound = vowelSearch[0].sound
        if(consonantSearch.length === 1) consonantSound = consonantSearch[0].sound

        if(swapSounds){
            currentSound = vowelSound + consonantSound
        }else{
            currentSound = consonantSound + vowelSound
        }
    }

    //util
    //https://bobbyhadz.com/blog/javascript-check-if-two-arrays-have-same-elements
    function areEqual(array1, array2) {
        if (array1.length === array2.length) {
            return array1.every(element => {
            if (array2.includes(element)) {
                return true;
            }

            return false;
            });
        }

        return false;
    }
</script>

<div>
    <svg width="300" height="441" xmlns="http://www.w3.org/2000/svg">
        <g>
            <title>Layer 1</title>
            <ellipse ry="7.90698" rx="7.90698" id="svg_13" cy="49.42857" cx="150" stroke="#000" fill="none"/>
            <ellipse ry="7.90698" rx="7.90698" id="svg_14" cy="100" cx="50" stroke="#000" fill="none"/>
            <ellipse ry="7.90698" rx="7.90698" id="svg_15" cy="100" cx="250" stroke="#000" fill="none"/>
            <ellipse ry="7.90698" rx="7.90698" id="svg_16" cy="150" cx="150" stroke="#000" fill="none"/>
            <ellipse ry="7.90698" rx="7.90698" id="svg_17" cy="250" cx="150" stroke="#000" fill="none"/>
            <ellipse ry="7.90698" rx="7.90698" id="svg_18" cy="300" cx="50" stroke="#000" fill="none"/>
            <ellipse ry="7.90698" rx="7.90698" id="svg_19" cy="300" cx="250" stroke="#000" fill="none"/>
            <ellipse ry="7.90698" rx="7.90698" id="svg_20" cy="350" cx="150" stroke="#000" fill="none"/>
            <ellipse on:click="{toggleSwapSounds}" ry="7.90698" rx="7.90698" id="swap_sounds" cy="400" cx="150" stroke={swapSounds ? "#F00" : "#000"} fill={swapSounds ? "#F00" : "#000"}/>
            <path stroke-width="6" id="svg_24" d="m287.38323,197.08282l-261.83586,0" opacity="undefined" stroke="#bf0000" fill="#000"/>
    
            {#each glpyhsParts as part, i}
                <line on:click="{handleClick}" id="{part.id}" y2="{part.y2}" x2="{part.x2}" y1="{part.y1}" x1="{part.x1}" stroke-width="12" stroke={part.checked ? "#F00" : "#000"} fill="none"/>
            {/each}
        </g>
    </svg>
    <br/>
    <button on:click="{handleDelete}">Delete</button>
</div>