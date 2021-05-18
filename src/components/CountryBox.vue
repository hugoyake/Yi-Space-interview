<template>
  <div class="country-box">
    <div v-if="isShow">
      <div class="modal" :class="{ 'is-active': modalStatus}">
        <div class="modal-background" @click="modalActive"></div>
            <div class="modal-content">
            <CountryDetail :detail="countryDetail" />
            </div>
        <button class="modal-close is-large" aria-label="close" @click="modalActive"></button>
      </div>
    </div>
    <div class="card">
        <div class="card-image">
        <figure class="image is-4by3">
            <img
            :src="country.flag"
            alt="Placeholder image"
            />
        </figure>
        </div>
        <div class="card-content">
        <div class="media">
            <div class="title-content">
            <p class="title is-4">{{ country.name }}</p>
            <p class="subtitle is-6">{{ country.nativeName }}</p>
            </div>
        </div>

        <div class="content">
            <div class="country-detail">
            <span class="detail-title">alpha2Code:</span>
            <p class="detail-content">{{ country.alpha2Code }}</p>
            </div>
            <div class="country-detail">
            <span class="detail-title">alpha3Code:</span>
            <p class="detail-content">{{ country.alpha3Code }}</p>
            </div>
            <div class="country-detail">
            <span class="detail-title">altSpellings:</span>
            <div class="detail-content">
                <p v-for="(item, index) in country.altSpellings" :key="index">{{ item }},</p>
            </div>
            </div>
            <div class="country-detail">
            <span class="detail-title">callingCodes:</span>
            <p class="detail-content" v-for="(item, index) in country.callingCodes"
    :key="index">{{ item }},</p>
            </div>
        </div>

        <button @click="modalActive" class="button is-primary" data-target="modal" aria-haspopup="true">More Detail</button>
        </div>
    </div>
  </div>
</template>
<script>
// import axios from 'axios'
import { ref } from 'vue'
// @ is an alias to /src
import CountryDetail from '@/components/CountryDetail.vue'

export default {
  name: 'CountryBox',
  components: {
    CountryDetail
  },
  props: {
    country: Object
  },
  setup (props) {
    const isShow = ref(false)
    const modalStatus = ref(false)
    const countryDetail = ref('')
    const modalActive = () => {
      isShow.value = !isShow.value
      modalStatus.value = !modalStatus.value
      countryDetail.value = props.country
    }
    return {
      isShow,
      modalActive,
      modalStatus,
      countryDetail
    }
  }
}
</script>

<style lang="sass">
.country-detail
  margin-bottom: 15px
  .detail-title
    display: block
    font-weight: bold
    text-align: left
  .detail-content
    display: block
    text-align: left
    p
      display: inline-block
      margin: 0
      text-align: left
.title-content
    width: 100%

@media screen and (max-width:768px)
  .modal-content
    width: calc(100% - 40px)
</style>
