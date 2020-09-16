<script>
  import CharityList from "../Components/CharityList.svelte";
  import Header from "../Components/Header.svelte";
  import Welcome from "../Components/Welcome.svelte";
  import Loader from "../Components/Loader.svelte";
  import Promo from "../Components/Promo.svelte";
  import Footer from "../Components/Footer.svelte";

  let title = "Charity";
  let data = getData();

  async function getData() {
    const res = await fetch("https://charity-api-bwa.herokuapp.com/charities");
    const data = await res.json();

    if (res.ok) {
      return data;
    } else {
      throw new Error(data);
    }
  }
</script>

<Header />
<Welcome />
{#await data}
  <Loader />
  <!-- #preloader -->
{:then charities}
  <CharityList {charities} />
{/await}

<Promo />
<Footer />
