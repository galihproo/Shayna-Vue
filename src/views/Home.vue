<template>
  <div class="home">
    <HeaderShayna />
    <HeroShayna />
    <HomeProductSlider />
    <InstaShayna />
    <PartnerLogo />
    <Footer />
  </div>
</template>

<script>
// @ is an alias to /src
// import HelloWorld from '@/components/HelloWorld.vue'
import HeaderShayna from '@/components/HeaderShayna.vue';
import HeroShayna from '@/components/HeroShayna.vue';
import HomeProductSlider from '@/components/HomeProductSlider.vue';
import InstaShayna from '@/components/InstaShayna.vue';
import PartnerLogo from '@/components/PartnerLogo.vue';
import Footer from '@/components/Footer.vue';

export default {
  name: 'Home',
  components: {
    HeaderShayna,
    HeroShayna,
    HomeProductSlider,
    InstaShayna,
    PartnerLogo,
    Footer
  }
}
</script>
