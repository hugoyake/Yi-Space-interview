<template>
  <div class="home container">
    <div class="columns mb-6">
      <!-- 反轉按鈕 -->
      <div class="column is-3">
        <div class="buttons">
          <button class="button is-primary button-customized" @click="reverseLists">{{ reverseButtonText }}</button>
        </div>
      </div>
      <!-- 搜尋欄 -->
      <div class="column search-bar-wrap">
        <div class="field has-addons">
            <div class="control search-bar">
                <input type="text" v-model="searchValue" class="input" placeholder="What are you looking for?"
                    name="query">
            </div>

            <div class="control">
                <button @click="searchActive" class="button is-success">
                    <span class="icon">
                        <i class="fas fa-search"></i>
                    </span>
                </button>
            </div>
        </div>
      </div>
      <!-- 「搜尋取消」按鈕 -->
      <div class="column is-2" v-if="cancelButtonStatus">
        <div class="buttons">
          <button class="button is-danger" @click="recoveredLists">Cancel</button>
        </div>
      </div>
    </div>

    <!-- 分頁 -->
    <nav class="pagination" role="navigation" aria-label="pagination">
      <a class="pagination-previous" @click="paginationOrevious">Previous</a>
      <a class="pagination-next" @click="paginationNext">Next page</a>
      <ul class="pagination-list"
          v-for="(item, index) in paginationLength"
          :key="item"
      >
        <li>
          <a @click="paginationSelect(index)"
             :class="{ 'is-current': index === paginationIsActive }"
             class="pagination-link"
             aria-label="Goto page 1"
          >
             {{index + 1}}
          </a>
        </li>
        <!-- <li>
          <span class="pagination-ellipsis">&hellip;</span>
        </li> -->
      </ul>
    </nav>

    <!-- 國家列表 -->
    <div class="columns country-box-wrap">
      <CountryBox
        class="column is-4 column-customized"
        v-for="(item, index) in countryListsShow"
        :key="index"
        :country="item" />
    </div>
  </div>
</template>
<script>
import axios from 'axios'
import { ref } from 'vue'
// @ is an alias to /src
import CountryBox from '@/components/CountryBox.vue'

export default {
  name: 'Home',
  components: {
    CountryBox
  },
  setup () {
    // 等會兒存放response.data
    let countryLists = []
    // reverse button 文字
    const reverseButtonText = ref('reverse order')
    // 分頁數量
    const paginationLength = ref('')
    // 顯示在page上的國家列表
    const countryListsShow = ref([])
    // 每頁數量
    const paginationNumber = 25
    // 分頁按鈕是否active
    const paginationIsActive = ref(0)
    // 搜尋目標
    const searchValue = ref('')
    // 「取消搜尋」按鈕是否顯示
    const cancelButtonStatus = ref(false)
    // 搜尋結果列表
    let searchArr = []

    /**
     * getCountryDetail()
     * axios
     */
    async function getCountryDetail () {
      await axios
        .get('https://restcountries.eu/rest/v2/all')
        .then((response) => {
          // response.data外溢
          countryLists = response.data
          // 顯示頁面
          pageStatus(response.data)
        })
        .catch((error) => {
          console.log(error)
        })
    }
    getCountryDetail()

    /**
     * pageStatus()
     * 頁面列表
     * @param  {Array}  lists    欲圖顯示的國家列表
     */
    const pageStatus = (lists) => {
      // 餘數
      const paginationRemainder = lists.length % paginationNumber
      if (paginationRemainder === 0) {
        // 餘數數為0
        paginationLength.value = lists.length / paginationNumber
      } else {
        // 餘數不完整
        paginationLength.value = parseInt(lists.length / paginationNumber) + 1
      }
      // 第一頁
      countryListsShow.value = lists.filter((item, index) => {
        return index < 25
      })
    }

    /**
     * paginationNext(), paginationOrevious()
     * 分頁下一頁，分頁上一頁
     */
    const paginationNext = () => {
      if (paginationIsActive.value + 2 <= paginationLength.value) {
        paginationSelect(paginationIsActive.value + 1)
      }
    }
    const paginationOrevious = () => {
      if (paginationIsActive.value - 1 >= 0) {
        paginationSelect(paginationIsActive.value - 1)
      }
    }

    /**
     * paginationSelect()
     * 分頁按鈕
     * @param  {Number}  index    第幾頁
     */
    const paginationSelect = (index) => {
      // 下方的filter也有index，所以要做區分
      const listsIndex = index
      // 顯示第index頁的頁面內容
      if (cancelButtonStatus.value === true) {
        countryListsShow.value = searchArr.filter((item, index) => {
          return index < 25 * (listsIndex + 1) && index >= 25 * (listsIndex)
        })
      } else {
        countryListsShow.value = countryLists.filter((item, index) => {
          return index < 25 * (listsIndex + 1) && index >= 25 * (listsIndex)
        })
      }
      // pagination按鈕active 效果
      paginationIsActive.value = index
    }

    /**
     * reverseLists()
     * 反轉序列
     */
    const reverseLists = () => {
      // 將頁面上的序列反轉
      countryListsShow.value = countryListsShow.value.reverse()
      // 按鈕上的文字
      if (reverseButtonText.value === 'reverse order') {
        reverseButtonText.value = 'ascending order'
      } else {
        reverseButtonText.value = 'reverse order'
      }
    }

    /**
     * searchActive()
     * 搜尋效果
     */
    const searchActive = () => {
      // 清空列表
      searchArr = []
      // 模糊搜尋
      for (var i = 0; i < countryLists.length; i++) {
        if (countryLists[i].name.indexOf(searchValue.value) >= 0) {
          // 將搜尋出來的結果push進arr
          searchArr.push(countryLists[i])
        }
      }
      // 顯示「取消搜尋」按鈕
      cancelButtonStatus.value = true
      // 頁面列表
      pageStatus(searchArr)
    }

    /**
     * recoveredLists()
     * 取消搜尋，回復成初始狀態
     */
    const recoveredLists = () => {
      // 隱藏「取消搜尋」按鈕
      cancelButtonStatus.value = false
      // 頁面列表
      pageStatus(countryLists)
      // pagination按鈕第一個active 效果
      paginationIsActive.value = 0
    }
    return {
      countryListsShow,
      paginationLength,
      paginationSelect,
      paginationIsActive,
      reverseLists,
      reverseButtonText,
      searchActive,
      searchValue,
      cancelButtonStatus,
      recoveredLists,
      paginationNext,
      paginationOrevious
    }
  }
}
</script>

<style lang="sass" scoped>
.country-box-wrap
  flex-wrap: wrap
.search-bar
  width: 100%
.search-bar-wrap
  flex-basis: auto
  width: 100%
.columns
  display: flex
.column-customized
  flex: none
  width: calc(100% / 4)
.button-customized
  width: 100%

@media screen and (max-width:1080px)
  .column-customized
    width: calc(100% / 3)

@media screen and (max-width:768px)
  .column-customized
    width: 50%

@media screen and (max-width:576px)
  .column-customized
    width: 100%
  .columns
    flex-wrap: wrap
  .button-customized
    width: 150px
</style>
