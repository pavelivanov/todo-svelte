<style>
  :global(body) {
    background-color: #f5f5f5;
  }

  .app {
    width: 400px;
    margin: 32px auto 0;
  }

  input {
    width: 100%;
    padding: 16px 32px;
    background-color: #fff;
    border: 0;
    font-weight: 300;
    font-size: 24px;
    box-sizing: border-box;
    box-shadow: 0 2px 4px 0 rgba(0, 0, 0, 0.2), 0 25px 50px 0 rgba(0, 0, 0, 0.1);
  }

  input::placeholder {
    color: #ddd;
    font-weight: 300;
    font-style: italic;
  }

  .content {
    padding: 20px 32px;
  }

  .items {
    margin-bottom: 32px;
  }

  .filters {
    display: flex;
  }

  .filter {
    margin-left: 12px;
    cursor: pointer;
  }

  .filter:first-child {
    margin-left: 0;
  }

  .filter.active {
    text-decoration: underline;
    cursor: default;
  }
</style>

<script>
  import Item from './Item.svelte'

  let inputValue = ''
  let items = []
  let filters = [ 'All', 'Completed', 'Active' ]
  let activeFilter = filters[0]
  let filteredItems

  const handleSubmit = (event) => {
    event.preventDefault()
    items = [ ...items, { title: inputValue } ]
    inputValue = ''
  }

  const handleFilterClick = (filter) => {
    activeFilter = filter
  }

  const handleItemRemove = (index) => {
    items = items.slice(0, index).concat(items.slice(index + 1))
  }

  $: filteredItems =
    activeFilter === 'All'
      ? items
      : activeFilter === 'Completed'
        ? items.filter((item) => item.isCompleted)
        : items.filter((item) => !item.isCompleted)
</script>

<div class="app">
  <form on:submit={handleSubmit}>
    <input bind:value={inputValue} placeholder="What needs to be done?" />
  </form>
  <div class="content">
    {#if filteredItems.length}
      <div class="items">
        {#each filteredItems as item, index}
          <Item data={item} onRemove={() => handleItemRemove(index)} />
        {/each}
      </div>
    {/if}
    <div class="filters">
      {#each filters as filter}
        <div
          class="filter"
          class:active={activeFilter === filter}
          on:click={() => handleFilterClick(filter)}
        >
          {filter}
        </div>
      {/each}
    </div>
  </div>
</div>
