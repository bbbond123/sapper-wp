<script>
  import PostList from "../components/PostList.svelte";
  import CategoryList from "../components/CategoryList.svelte";
</script>

<svelte:head>
  <title>Sapper wordpress site</title>
</svelte:head>

<div class="cats-posts">
  <CategoryList />

  <div class="posts">
    <PostList />
  </div>
</div>


<style lang="scss">
	
</style>