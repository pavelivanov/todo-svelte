<style>
  div {
    display: flex;
    justify-content: space-between;
    margin-top: 16px;
    font-size: 20px;
    cursor: pointer;
  }

  div:first-child {
    margin-top: 0;
  }

  div.completed .title {
    text-decoration: line-through;
  }

  div:hover .close {
    display: block;
  }

  .close {
    display: none;
    cursor: pointer;
  }
</style>

<script>
  export let data = { title: '', isCompleted: false }
  export let onRemove

  const handleClick = () => {
    data.isCompleted = !data.isCompleted
  }

  const handleRemove = (event) => {
    event.stopPropagation()
    onRemove()
  }
</script>

<div class:completed={data.isCompleted} on:click={handleClick}>
  <span class="title">{data.title}</span>
  <span class="close" on:click={handleRemove}>&times;</span>
</div>
