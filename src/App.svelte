<script>
import FancyList from './Child.svelte'
import { v4 as uuid } from 'uuid'
import { tick } from 'svelte';

let fancy_list
	let items = [{id:1,text:'Apple',add_to_cart:true}, {id:2,text:'Orange',add_to_cart:false}, {id:3,text:'Grapes',add_to_cart:true}]
    
    console.log(uuid());

	async function handle_create_fruit(evt){
		
        let new_fruit_object = evt.detail
        new_fruit_object.id = uuid()
		
       console.log(document.querySelectorAll('.list_items ul li'))
        items = [...items,new_fruit_object]
        fancy_list.clear_input_text()
        fancy_list.focus_input()
        console.log(items);
        await tick()
        console.log(document.querySelectorAll('.list_items ul li'))
	}

    function handle_remove_fruit(evt){
        items = items.filter((single_item)=>single_item.id!=evt.detail)
    }
   
	

	
let show_list =true
</script>
{fancy_list?.read_only}
{items.length}
{#if show_list}
<div class="list_items">
    <FancyList bind:this={fancy_list} {items} on:add_fruit={handle_create_fruit} on:remove={handle_remove_fruit}>
      
    </FancyList>
</div>
{/if}
<button on:click={()=>console.log(items)}>check array</button>



<button on:click={()=>{show_list=!show_list}}>hide</button>



<style>
    .list_items{
        max-width: 300px;
    }
</style>