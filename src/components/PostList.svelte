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

  onMount(async () => {
    const res = await fetch(`${apiURL}/wp/v2/posts?per_page=5`);
    const json = await res.json();
    posts = json;
  });
</script>

<ul>
  {#each posts as post}
    <li>
      <a rel="prefetch" href={`articles/${post.slug}`}>{post.title.rendered}</a>
    </li>
  {/each}
</ul>
