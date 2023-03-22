<script setup lang="ts">
import sliderBar1 from '@images/illustrations/sidebar-pic-1.png';
import sliderBar2 from '@images/illustrations/sidebar-pic-2.png';
import sliderBar3 from '@images/illustrations/sidebar-pic-3.png';
import { VIcon } from 'vuetify/components';
import stepper from './stepper.vue';

const websiteAnalytics = [
  {
    name: 'Traffic',
    slideImg: sliderBar1,
    data: [
      {
        number: '1.5k',
        text: 'Sessions',
      },
      {
        number: '3.1k',
        text: 'Page Views',
      },
      {
        number: '1.2k',
        text: 'Leads',
      },
      {
        number: '12%',
        text: 'Conversions',
      },
    ],
  },
  {
    name: 'Spending',
    slideImg: sliderBar2,
    data: [
      {
        number: '12h',
        text: 'Spend',
      },
      {
        number: '182',
        text: 'Order Size',
      },
      {
        number: '127',
        text: 'Order',
      },
      {
        number: '23k',
        text: 'Items',
      },
    ],
  },
  {
    name: 'Revenue Sources',
    slideImg: sliderBar3,
    data: [
      {
        number: '268',
        text: 'Direct',
      },
      {
        number: '890',
        text: 'Organic',
      },
      {
        number: '622',
        text: 'Referral',
      },
      {
        number: '1.2k',
        text: 'Campaign',
      },
    ],
  },
]
</script>

<template>
  <VRow no-gutters class="auth-wrapper">
    <VCol lg="6" class="d-none d-lg-flex">
      <div class="position-relative auth-bg rounded-lg w-100 me-0">
        <div class="d-flex align-center justify-center w-100 carousel ">
        
          
          <VCarousel
          cycle
          :continuous="false"
          :show-arrows="false"
          hide-delimiter-background
          :delimiter-icon="() => h(VIcon, { icon: 'fa-minus', size: '15' })"
          height="auto"
          class="carousel-delimiter-top-end web-analytics-carousel"
        >
         <VCarouselItem>
          <VImg  src="https://source.unsplash.com/collection/190727/1600x900/"/>
         </VCarouselItem>
         <VCarouselItem>
          <VImg src="https://source.unsplash.com/collection/190727/1600x900/"/>
         </VCarouselItem>
         <VCarouselItem>
          <VImg src="https://source.unsplash.com/collection/190727/1600x900/"/>
         </VCarouselItem>
        </VCarousel>
      
        </div>
      </div>
    </VCol>

    <VCol cols="12" lg="6" class="d-flex align-center justify-center">

      <VCol flat class="mt-12 mt-sm-0 pa-4">
        <VCardText>
          <h6 class="text-h5 mb-3">
            Let's turn your messy mailbox into identified users and unique
            releases! 🤩
          </h6>
          <p class="card-text mb-0">
            Create your account and get access the world\'s largest catalog of
            unreleased music!
          </p>
        </VCardText>
        <stepper/>
      </VCol>
    </VCol>
  </VRow>
  
</template>

<style lang="scss">
.card-website-analytics-img {
  block-size: 160px;
}

element.style {
  left: auto;
  right: auto;
}

.carousel-delimiter-top-end .v-carousel__controls{
  justify-content: center !important;
  top: auto !important;
    /* padding-inline: 1rem; */
}
.carousel{
  height: 100vh !important;
  background: white;
  
}


@media screen and (min-width: 600px) {
  .card-website-analytics-img {
    position: absolute;
    margin: auto;
    inset-block-end: 40px;
    inset-block-start: -1rem;
    inset-inline-end: 1rem;
  }
}

.web-analytics-carousel {
  .v-carousel__controls {
    .v-btn:not(.v-btn--active) {
      opacity: 0.4;
    }
  }
}
.app-customizer-toggler{
  display: none !important;
} 
</style>

