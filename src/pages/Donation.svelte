<script>
  import router from "page";
  import { onMount } from "svelte";
  import Header from "../Components/Header.svelte";
  import Loader from "../Components/Loader.svelte";
  import Footer from "../Components/Footer.svelte";

  export let params;
  let charity,
    amount,
    name,
    email,
    agree = false;
  let data = getCharity(params.id);

  async function getCharity(id) {
    const res = await fetch(
      `https://charity-api-bwa.herokuapp.com/charities/${id}`
    );
    return res.json();
  }

  async function formSubmit(event) {
    charity.pledged = charity.pledged + parseInt(amount);
    try {
      const res = await fetch(
        `https://charity-api-bwa.herokuapp.com/charities/${params.id}`,
        {
          method: "PUT",
          headers: {
            "content-type": "application/json",
          },
          body: JSON.stringify(charity),
        }
      );
      console.log(res);
      router.redirect("/success");
    } catch (err) {
      console.log(err);
    }
  }
</script>

<style>
  #xs-input-checkbox {
    display: flex;
    align-items: center;
  }

  #xs-donate-agree {
    width: 35px;
  }

  label[for="xs-donate-agree"] {
    margin: 0;
    margin-left: 10px;
  }

  .xs-donation-form-images {
    text-align: center;
  }
</style>

<Header />

{#await data}
  <Loader />
{:then charity}
  <section
    class="xs-banner-inner-section parallax-window"
    style="background-image:url('/assets/images/p.jpg')">
    <div class="xs-black-overlay" />
    <div class="container">
      <div class="color-white xs-inner-banner-content">
        <h2>Donate Now</h2>
        <p>{charity.title}</p>
        <ul class="xs-breadcumb">
          <li class="badge badge-pill badge-primary">
            <a href="/" class="color-white">Home /</a> Donate
          </li>
        </ul>
      </div>
    </div>
  </section>
  <!--breadcumb end here-->
  <!-- End welcome section -->
  <main class="xs-main">
    <!-- donation form section -->
    <section class="xs-section-padding bg-gray">
      <div class="container">
        <div class="row">
          <div class="col-lg-6">
            <div class="xs-donation-form-images">
              <img
                src={charity.thumbnail}
                class="img-responsive"
                alt="Family Images" />
            </div>
          </div>
          <div class="col-lg-6">
            <div class="xs-donation-form-wraper">
              <div class="xs-heading xs-mb-30">
                <h2 class="xs-title">{charity.title}</h2>
                <p class="small">
                  To learn more about make donate charity with us visit our "<span
                    class="color-green">Contact us</span>" site. By calling <span
                    class="color-green">+44(0) 800 883 8450</span>.
                </p><span class="xs-separetor v2" />
              </div>
              <!-- .xs-heading end -->
              <form
                action="#"
                on:submit|preventDefault={formSubmit}
                method="post"
                id="xs-donation-form"
                class="xs-donation-form"
                name="xs-donation-form">
                <div class="xs-input-group">
                  <label for="xs-donate-name">Your Donation <span class="color-light-red">**</span></label>
                  <input
                    type="text"
                    name="amount"
                    id="xs-donate-name"
                    bind:value={amount}
                    required
                    class="form-control"
                    placeholder="Your Donation in Rupiah" />
                </div>
                <div class="xs-input-group">
                  <label for="xs-donate-name">Your Name <span class="color-light-red">**</span></label>
                  <input
                    type="text"
                    name="name"
                    id="xs-donate-name"
                    required
                    bind:value={name}
                    class="form-control"
                    placeholder="Your Awesome Name" />
                </div>
                <div class="xs-input-group">
                  <label for="xs-donate-name">Your Email <span class="color-light-red">**</span></label>
                  <input
                    type="email"
                    name="email"
                    id="xs-donate-name"
                    class="form-control"
                    required
                    bind:value={email}
                    placeholder="Alamat Email Anda" />
                </div>
                <div class="xs-input-group" id="xs-input-checkbox">
                  <input
                    type="checkbox"
                    bind:checked={agree}
                    name="agree"
                    id="xs-donate-agree" />
                  <label for="xs-donate-agree">
                    I Agree <span class="color-light-red"> ** </span>
                  </label>
                </div>
                <button
                  type="submit"
                  disabled={!agree}
                  class="btn btn-warning"><span class="badge"><i
                      class="fa fa-heart" /></span> Donate now</button>
              </form>
              <!-- .xs-donation-form #xs-donation-form END -->
            </div>
          </div>
        </div>
        <!-- .row end -->
      </div>
      <!-- .container end -->
    </section>
    <!-- End donation form section -->
  </main>
{/await}
<!-- footer section start -->
<Footer />
