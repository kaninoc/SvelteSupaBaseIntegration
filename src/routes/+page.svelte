<script>

    import { StorageClient } from '@supabase/storage-js'
    import { onMount } from "svelte";
    import { createClient } from '@supabase/supabase-js'
    import Todo from "$lib/Todo.svelte"

    let url = ''
    let key = ''

    // Create a single supabase client for interacting with your database
    const supabase = createClient(url, key)

    let allProducts = []
    let newProductName ="";
    let newProductDescription="";

    onMount(async ()=>{
        await getAllProducts();
    });

    const addNewProduct = async() =>{
        try{
            
            const { data, error } = await supabase
            .from('ecoflowproducts')
            .insert([{ name: newProductName, description: newProductDescription }])
            .select();
            await getAllProducts();
            newProductName ="";
            newProductDescription ="";
        }catch(err){
            console.log(err)
        }
    }

    const getAllProducts = async() =>{
        try{
            let { data, error } = await supabase.from('ecoflowproducts').select('*')
            allProducts = data;
            console.table(allProducts);

        }catch(err){
            console.log(err)
        }
    }

    const updateProduct = async(todo) =>{
        try{
            
            const { data, error } = await supabase
            .from('ecoflowproducts')
            .update({ name: todo.name ,description: todo.description})
            .eq('id', todo.id);
            await getAllProducts();
        }catch(err){
            console.log(err)
        }
    }

    const deleteProduct = async(todo) =>{
        try{        
            
            const { error } = await supabase
            .from('ecoflowproducts')
            .delete()
            .eq('id', todo.id);

            await getAllProducts();
        }catch(err){
            console.log(err)
        }
    }


</script>

<!-- input text new product-->
<div class="add-todo">
    <div>
        <label for="0.25em">Name</label>
        <input type="text" bind:value ={newProductName}>
    </div>
    <div>
        <label for="0.25em">Description</label>
        <input type="text" bind:value ={newProductDescription}>
        <button on:click={ () => addNewProduct()}>Add Task</button>
    </div>
    
</div>

<!-- List element -->
{#each allProducts as todo}
    <Todo {todo}{updateProduct}{deleteProduct}/>
{:else}
<p>No products found</p>
{/each}

<style>
    .add-todo{
        display: flex;
        margin-bottom: 0.5em;
    }
</style>