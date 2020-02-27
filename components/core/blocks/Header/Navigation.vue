<template>
  <section class="section  section__nav">
    <div class="container">
      <nav class="navigation">
        <ul>
          <li v-for="(links, index) in nav" :key="index">
            <router-link :to=links.link >
              {{links.title}}
            </router-link>
          </li>
        </ul>
      </nav>
    </div>
  </section>
</template>

<script>
export default {
  data() {
    return{
      nav:[
        {
          title: "Portofoons",
          link: "/",
        },
        {
          title: "Accessoires",
          link: "/gear/gear-4",
        },
        {
          title: "Zendontvangers",
          link: "/men/men-5",
        },
        {
          title: "Antennes & Toebehoren ",
          link: "/training/training-3",
        },
        {
          title: "Contact",
          link: "/contact",
        },
        {
          title: "Smartportofoon",
          link: "/sale",
        },
        {
          title: "Nieuws",
          link: "/magazine",
        }
      ]
    }
  }
}
</script>

<style>

</style>
