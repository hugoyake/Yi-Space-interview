<template>
   <div class="country-detail">
     <div class="card">
      <div class="card-image">
        <figure class="image is-4by3">
          <img :src="detail.flag" alt="" srcset="">
        </figure>
      </div>
      <div class="card-content">
        <div class="media">
          <div class="title-content">
            <p class="title is-4">{{ detail.name }}</p>
            <p class="subtitle is-6">{{ detail.nativeName }}</p>
          </div>
        </div>
        <div class="content">
          <div class="country-detail">
            <span class="detail-title">alpha2Code:</span>
            <p class="detail-content">{{ detail.alpha2Code }}</p>
          </div>

          <div class="country-detail">
            <span class="detail-title">alpha3Code:</span>
            <p class="detail-content">{{ detail.alpha3Code }}</p>
          </div>

          <div class="country-detail">
            <span class="detail-title">altSpellings:</span>
            <div class="detail-content">
              <p v-for="(item, index) in detail.altSpellings" :key="index">{{ item }},</p>
            </div>
          </div>

          <div class="country-detail">
            <span class="detail-title">area:</span>
            <p class="detail-content">{{ detail.area }}</p>
          </div>

          <div class="country-detail">
            <span class="detail-title">borders:</span>
            <div class="detail-content">
              <p v-for="(item, index) in detail.borders" :key="index">{{ item }},</p>
            </div>
          </div>
          <div class="country-detail">
            <span class="detail-title">callingCodes:</span>
            <div class="detail-content">
              <p v-for="(item, index) in detail.callingCodes" :key="index">
                {{ item }},
              </p>
            </div>
          </div>

          <div class="country-detail">
            <span class="detail-title">capital:</span>
            <p class="detail-content">{{ detail.capital }}</p>
          </div>

          <div class="country-detail">
            <span class="detail-title">cioc:</span>
            <p class="detail-content">{{ detail.cioc }}</p>
          </div>
          <div class="country-detail">
            <span class="detail-title">currencies:</span>
            <div class="detail-content">
              <p v-for="(item, index) in detail.currencies" :key="index">
                {{ item.name }},
              </p>
            </div>
          </div>

          <div class="country-detail">
            <span class="detail-title">demonym:</span>
            <p class="detail-content">{{ detail.demonym }}</p>
          </div>

          <div class="country-detail">
            <span class="detail-title">gini:</span>
            <p class="detail-content">{{ detail.gini }}</p>
          </div>

          <div class="country-detail">
            <span class="detail-title">languages:</span>
            <div class="detail-content">
              <p v-for="(item, index) in detail.languages" :key="index">
                {{ item.name }},
              </p>
            </div>
          </div>
          <div class="country-detail">
            <span class="detail-title">latlng:</span>
            <div class="detail-content">
              <p v-for="(item, index) in detail.latlng" :key="index">
                {{ item.name }},
              </p>
            </div>
          </div>

          <div class="country-detail">
            <span class="detail-title">numericCode:</span>
            <p class="detail-content">{{ detail.numericCode }}</p>
          </div>

          <div class="country-detail">
            <span class="detail-title">population:</span>
            <p class="detail-content">{{ detail.population }}</p>
          </div>

          <div class="country-detail">
            <span class="detail-title">region:</span>
            <p class="detail-content">{{ detail.region }}</p>
          </div>

          <div class="country-detail">
            <span class="detail-title">regionalBlocs:</span>
            <div class="detail-content">
              <p v-for="(item, index) in detail.regionalBlocs" :key="index">
                {{ item.name }},
              </p>
            </div>
          </div>
          <div class="country-detail">
            <span class="detail-title">subregion:</span>
            <p class="detail-content">{{ detail.subregion }}</p>
          </div>

          <div class="country-detail">
            <span class="detail-title">timezones:</span>
            <div class="detail-content">
              <p v-for="(item, index) in detail.timezones" :key="index">
                {{ item.name }},
              </p>
            </div>
          </div>

          <div class="country-detail">
            <span class="detail-title">topLevelDomain:</span>
            <div class="detail-content">
              <p v-for="(item, index) in detail.topLevelDomain" :key="index">
                {{ item.name }},
              </p>
            </div>
          </div>

          <div class="country-detail">
            <span class="detail-title">translations:</span>
            <div class="detail-content">
              <p v-for="(item, index) in detail.translations" :key="index">
                {{ item.name }},
              </p>
            </div>
          </div>
        </div>
      </div>
    </div>
   </div>
</template>

<script>
export default {
  name: 'CountryDetail',
  props: {
    detail: Object
  },
  setup (props) {
    console.log(props)
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="sass">

</style>
