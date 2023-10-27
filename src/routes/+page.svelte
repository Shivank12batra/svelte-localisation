<script lang="ts">
    import { Router, Route, Link } from "svelte-routing";
    import {
        _,
        getLocaleFromNavigator,
        isLoading,
        register,
        init,
        locale
    } from "svelte-i18n";

    // register function to only load the locale when user selects it
    register("en", () => import("../static/locales/en.json"));
    register("es", () => import("../static/locales/es.json"));

    init({
        fallbackLocale: "en",
        initialLocale: getLocaleFromNavigator()
    });

    const handleLocaleChange = (e) => {
        e.preventDefault();
        locale.set(e.target!.value);
    };
</script>

{#if $isLoading}
  <p>Loading</p>
{:else}
  <main>
    <Router>
      <select on:change={handleLocaleChange}>
        <option value="en">en</option>
        <option value="es">es</option>
      </select>
      <header >
        <nav>
          <Link to="/">{$_('nav.home')}</Link>
          <Link to="features">{$_('nav.features')}</Link>
          <Link to="about">{$_('nav.about')}</Link>
        </nav>
      </header>
      <section>
        <Route path="/">
          <h3>{$_('home.title')}</h3>
          <p>{$_('home.description')}</p>
        </Route>
        <Route path="features">
          <h3>{$_('features.title')}</h3>
          <p>{$_('features.description')}</p>
        </Route>
        <Route path="about">
        <h3>{$_('about.title')}</h3>
          <p>{$_('about.description')}</p>
        </Route>
      </section>
    </Router>
  </main>
{/if}

<style>
    @import url("https://fonts.googleapis.com/css2?family=Montserrat&display=swap");
  
    * {
      padding: 0;
      margin: 0;
      box-sizing: border-box;
      font-family: "Montserrat", sans-serif;
    }
  
    :global(body) {
      background-image: linear-gradient(120deg, #d4fc79 0%, #96e6a1 100%);
      min-height: 100vh;
      color: black;
      padding: 10px;
    }
  
    main {
      max-width: 600px;
      margin: 0 auto;
    }
  
    select {
      border: none;
      padding: 10px;
      margin-bottom: 20px;
      border-radius: 5px;
    }
  
    nav {
      margin-bottom: 20px;
      display: grid;
      grid-template-columns: 1fr 1fr 1fr;
      text-align: center;
      gap: 20px;
    }
  
    nav > :global(a) {
      background-color: white;
      padding: 10px 20px;
      border-radius: 5px;
      color: black;
      font-weight: bold;
      text-decoration: none;
    }
  
    section {
      background-color: white;
      padding: 20px;
      border-radius: 5px;
    }
  
    h3 {
      margin-bottom: 10px;
    }
  
    @media screen and (max-width: 500px) {
      nav {
        grid-template-columns: 1fr;
      }
    }
</style>
