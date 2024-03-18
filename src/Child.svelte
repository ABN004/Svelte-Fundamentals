<svelte:options immutable= {true}/>
<script>
	import { createEventDispatcher,beforeUpdate,afterUpdate } from 'svelte'
	export let items
	let auto_scroll
	let previous_items = items
	$:{
        auto_scroll = items.length > previous_items.length
        previous_items = items
    }
	let input_text =''
	export const read_only = 'read only'
	export function clear_input_text(){
		input_text = ''
	}
	export function focus_input(){
		input.focus()
	}
	let input,list_ul
	let dispatch = createEventDispatcher()

	function handle_add_fruits(){

		let is_prevent = dispatch('add_fruit',{text:input_text},{cancelable:true}) //cancellable:true is used for giving preventdefault usage in parent component
			// is_prevent is a boolean, which give false or true value when the parent component calls preventDefault or not 
		
	}

	function handle_remove_todo(id){
		dispatch('remove',id)
	}
	
	beforeUpdate(()=>{
		console.log(list_ul?.offsetHeight);
	})
	afterUpdate(()=>{
		if(auto_scroll){
			list_ul.scrollTo(0,list_ul.scrollHeight)
		}
		auto_scroll = false
		
	})
</script>

<ul bind:this={list_ul}>
	{#each items as item}
	{(console.log(item),'')}
	<li class="fancy">
		<label for="">
			<input type="checkbox" name="" id=""  bind:checked={item.add_to_cart}>
			<span>{item.text}</span>
		</label>
		<button on:click={()=>handle_remove_todo(item.id)}>remove</button>
	</li>
	{/each}
</ul>
<form on:submit|preventDefault={handle_add_fruits}>
	<input type="text" bind:this={input} bind:value={input_text}>
	<button type="submit" disabled={!input_text}>Add Fruit</button>
</form>


<style>
	ul li label{
		display: inline-block;
	}
	ul{
		max-height: 150px;
		overflow: auto;
	}
</style>