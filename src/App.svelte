<script>
	import * as glyphJson from './static/glyph-info.json';
    import * as languageJson from './static/language.json';

	import Glyph from "./Glyph.svelte";


	let glyphs = []

	function addGlyph(ev){
		glyphs = [...glyphs, {currentSound: ""}]
		console.log(glyphs);

		console.log(glyphs.filter(x => x.currentSound !== ""))
	}

	function removeGlyph(glyphToRemove){
		glyphs = glyphs.filter(x => x !== glyphToRemove)
	}

	function addSpace(){
		glyphs = [...glyphs, {currentSound: " "}]
	}

	$: currentSound = glyphs.filter(x => x.currentSound !== "").map(x => x.currentSound).join("");
</script>

<main>
	<div class="current-sound">{currentSound === "" ? `_` : currentSound}</div>
	<div>		
		<button on:click="{addGlyph}">Add Glyph</button>
		<button on:click="{addSpace}">Add Space</button>
	</div>
	<div>
		{#each glyphs as glyph, i}
			{#if glyph.currentSound == " "}
				<button on:click="{removeGlyph(glyph)}">Remove Space</button><br/>
			{:else}
				<Glyph bind:currentSound={glyph.currentSound} on:delete={removeGlyph(glyph)} glyphJson={glyphJson} languageJson={languageJson}/>
		 	{/if}
		{/each}
	</div>
</main>

<style>
	main {
		text-align: center;
		padding: 1em;
		max-width: 240px;
		margin: 0 auto;
	}

	h1 {
		color: #ff3e00;
		text-transform: uppercase;
		font-size: 4em;
		font-weight: 100;
	}

	.current-sound{
		padding-bottom: 10px;
		text-transform: uppercase;
		font-size: 2em;
		font-weight: 100;
	}

	@media (min-width: 640px) {
		main {
			max-width: none;
		}
	}
</style>