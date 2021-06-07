<script>
  import { onMount } from "svelte";

  let posts = [];
  let totalPages = null;
  let currentPage = 1;
  let pages = [];

  const apiURL = process.env.SAPPER_APP_API_URL;

  function createPagesArray(total) {
    let arr = [];
    for (let i = 1; i <= total; i++) {
      arr.push(i);
    }

    return arr;
  }

  function changePage(page) {
    fetch(`${apiURL}/wp/v2/posts?per_page=${page}`)
      .then((res) => {
        return res.json();
      })
      .then((result) => {
        posts = result;
        currentPage = page;
      });
  }

  onMount(async () => {
    const res = await fetch(`${apiURL}/wp/v2/posts?per_page=5`);
    const json = await res.json();

    totalPages = res.headers.get("X-WP-TotalPages");
    pages = createPagesArray(totalPages);
    console.log("🚀 ~ file: PostList.svelte ~ line 26 ~ onMount ~ pages", pages);

    posts = json;
  });
</script>

<ul class="post-list">
  {#each posts as post}
    <li>
      <a rel="prefetch" href={`articles/${post.slug}`}>{post.title.rendered}</a>
    </li>
  {/each}
</ul>
<ul class="pagination">
  {#each pages as page}
    <li>
      <button
        on:click={() => {
          console.log(page);
          changePage(page);
        }}
      >
        {page}
      </button>
    </li>
  {/each}
</ul>

<style>
  /* your styles go here */
</style>
