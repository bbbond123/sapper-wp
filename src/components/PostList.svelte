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
    fetch(`${apiURL}/wp/v2/posts?per_page=5&page=${page}`)
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
        class={page === currentPage ? "active" : ""}
        on:click={() => {
          changePage(page);
        }}
      >
        {page}
      </button>
    </li>
  {/each}
</ul>

<style lang="scss">
  .pagination,
  .post-list {
    margin: 0;
    padding: 0;
    list-style: none;
  }

  .pagination {
    display: flex;
    margin-top: 40px;

    li {
      margin-right: 10px;

      &:last-of-type {
        margin-right: 0;
      }
    }

    button {
      padding: 10px 15px;
      border: 1px solid #cccccc;
      cursor: pointer;

      &.active {
        cursor: pointer;
        background: #ff3e00;
        color: #FFFFFF;
      }
    }
  }
</style>
