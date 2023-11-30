<script>
    export let todo, updateProduct,deleteProduct;
</script>

<div class="todos">
    <input type="text" value={todo.name} on:input={(e) =>{
        todo.name = e.currentTarget.value;
        updateProduct(todo)}}/>
    <input type="text" value={todo.description} on:input={(e) =>{
        todo.description = e.currentTarget.value;
        updateProduct(todo)}}/>
    <input type="text" value={todo.created_at}/>
    <input type="text" value={todo.userCreate}/>

    <button on:click={() =>deleteProduct(todo)}>Delete</button>
</div>


<style>
    .todos{
        display: flex;
        margin-bottom: 0.25em;
    }
</style>