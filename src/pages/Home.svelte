<script>
  import CharityList from "../Components/CharityList.svelte";
  import Header from "../Components/Header.svelte";
  import Welcome from "../Components/Welcome.svelte";
  import Promo from "../Components/Promo.svelte";
  import Footer from "../Components/Footer.svelte";
  import { charities } from "../data/charities";

  let title = "Charity";
</script>

<Header />
<Welcome />
<CharityList {charities} />
<Promo />
<Footer />
