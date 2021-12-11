<script lang="ts">
  import {
    Button,
    Collapse,
    Dropdown,
    DropdownItem,
    DropdownMenu,
    DropdownToggle,
    Nav,
    Navbar,
    NavbarBrand,
    NavbarToggler,
    NavItem,
    NavLink,
  } from "sveltestrap";
  import Router from "svelte-spa-router";
  import Home from "./Home.svelte";
  import SearchByCategory from "./SearchByCategory.svelte";
  import MovieDetails from "./MovieDetails.svelte";
  import SearchByName from "./SearchByName.svelte";
  import SearchByActor from "./SearchByActor.svelte";

  export let name: string;
  export let isOpen = false;
</script>

<svelte:head>
  <link
    rel="stylesheet"
    href="https://cdn.jsdelivr.net/npm/bootstrap@5.1.0/dist/css/bootstrap.min.css"
  />
</svelte:head>

<main style="margin:0px;padding:0px">
  <Navbar dark expand="md" style="background-color:seagreen" class="sticky-top">
    <NavbarBrand href="/">Movie App</NavbarBrand>
    <NavbarToggler on:click={() => (isOpen = !isOpen)} />
    <Collapse
      {isOpen}
      navbar
      expand="md"
      on:update={() => {
        isOpen = !isOpen;
      }}
    >
      <Nav class="overflow-auto" navbar>
        <NavItem>
          <NavLink href="#/">Home</NavLink>
        </NavItem>
        <NavItem>
          <NavLink href="#/search-by-name/">Search by Name</NavLink>
        </NavItem>
        <NavItem>
          <NavLink href="#/search-by-category/">Search by Category</NavLink>
        </NavItem>
        <NavItem>
          <NavLink href="#/search-by-actor/">Search Actor</NavLink>
        </NavItem>
      </Nav>
    </Collapse>
  </Navbar>

  <Router
    routes={{
      "/": Home,
      "/moviedetail/:id": MovieDetails,
      "/search-by-name": SearchByName,
      "/search-by-category": SearchByCategory,
      "/search-by-actor": SearchByActor,
    }}
  />
</main>
