<script>
  import { onMount } from "svelte";
  let categories = [];

  const apiUrl = process.env.SAPPER_APP_API_URL;

  onMount(async () => {
    const res = await fetch(`${apiUrl}/wp/v2/categories?per_page=100`);
    categories = await res.json();
  });
</script>

<ul class="cats post-list">
  {#each categories as category}
    <li>
      <a href={`categories/${category.slug}`}>
        {category.name}
      </a>
    </li>
  {/each}
</ul>
