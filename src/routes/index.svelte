<script context="module">
  export async function preload({ params, query }) {
    const authRes = await axios.post(
      "https://strapi-mra-test.herokuapp.com/auth/local",
      {
        identifier: "test@test.com",
        password: "test1234"
      }
    );
    const token = authRes.data.jwt;
    const pageData = await axios.get(
      "https://strapi-mra-test.herokuapp.com/pages",
      {
        headers: {
          Authorization: `Bearer ${token}`
        }
      }
    );

    return {
      content: pageData.data[0].content
    };
  }
</script>

<script>
  import { onMount } from "svelte";
  import axios from "axios";
  export let content = "";
  let photo = "";
  onMount(async () => {
    const res = await fetch(
      `https://jsonplaceholder.typicode.com/photos?_limit=20`
    );
    const photos = await res.json();
    photo = photos[0].title;
  });
</script>

<style>
  h1,
  figure,
  p {
    text-align: center;
    margin: 0 auto;
  }

  h1 {
    font-size: 2.8em;
    text-transform: uppercase;
    font-weight: 700;
    margin: 0 0 0.5em 0;
  }

  figure {
    margin: 0 0 1em 0;
  }

  img {
    width: 100%;
    max-width: 400px;
    margin: 0 0 1em 0;
  }

  p {
    margin: 1em auto;
  }

  @media (min-width: 480px) {
    h1 {
      font-size: 4em;
    }
  }
</style>

<svelte:head>
  <title>Sapper project template</title>
</svelte:head>

<h1>Great success!</h1>

<figure>
  <img alt="Borat" src="great-success.png" />
  <figcaption>HIGH FIVE!</figcaption>
</figure>

<p>
  <strong>
    {@html content}
  </strong>
</p>

<p>{photo}</p>
