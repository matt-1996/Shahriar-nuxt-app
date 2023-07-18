<template>
  <div>
    <v-container>
      <v-row>
        <v-col>
          <v-text-field
          label="نام و نام خانوادگی"
          v-model="name"
          required
          :rules="RequiredRules"
          ></v-text-field>
          <v-btn block @click="search" :disabled="isLoading == true" type="submit">جستجو</v-btn>
        </v-col>
      </v-row>
      <v-row>
        <v-col>
          <div v-if="responseData">
            <!-- <div v-for="res in responseData" :key="res.id">
            </div> -->
            <div class="tw-flex tw-flex-col">
  <div class="tw-overflow-x-auto tw-sm:-mx-6 tw-lg:-mx-8">
    <div class="tw-inline-block tw-min-w-full tw-py-2 tw-sm:px-6 tw-lg:px-8">
      <div class="tw-overflow-hidden">
        <table class="tw-min-w-full tw-text-center tw-text-sm tw-font-light">
          <thead
            class="tw-border-b tw-bg-gray-900 tw-font-medium tw-dark:border-neutral-500 tw-dark:bg-neutral-600">
            <tr>
              <th scope="col" class="tw-px-6 tw-py-4">نام و نام خانوادگی</th>
              <th scope="col" class="tw-px-6 tw-py-4">نام پدر</th>
              <th scope="col" class="tw-px-6 tw-py-4">تاریخ ولادت</th>
              <th scope="col" class="tw-px-6 tw-py-4">تاریخ فوت</th>
              <th scope="col" class="tw-px-6 tw-py-4">فاز</th>
              <th scope="col" class="tw-px-6 tw-py-4">قطعه</th>
              <th scope="col" class="tw-px-6 tw-py-4">نام قطعه</th>
              <th scope="col" class="tw-px-6 tw-py-4">ردیف</th>
              <th scope="col" class="tw-px-6 tw-py-4">شماره</th>
              <th scope="col" class="tw-px-6 tw-py-4">طبقه</th>
              <th scope="col" class="tw-px-6 tw-py-4">محل دفن</th>
            </tr>
          </thead>
          <tbody>
            <tr
            v-for="res in responseData" :key="res.id"
              class="tw-border-b tw-bg-gray-900 tw-dark:border-neutral-500 tw-dark:bg-neutral-700">
              <td class="tw-whitespace-nowrap tw-px-6 tw-py-4 tw-font-medium">{{res.fullName}}</td>
              <td class="tw-whitespace-nowrap tw-px-6 tw-py-4 tw-font-medium">{{res.father}}</td>
              <td class="tw-whitespace-nowrap tw-px-6 tw-py-4 tw-font-medium">{{res.born}}</td>
              <td class="tw-whitespace-nowrap tw-px-6 tw-py-4 tw-font-medium">{{res.death}}</td>
              <td class="tw-whitespace-nowrap tw-px-6 tw-py-4 tw-font-medium">{{res.faz}}</td>
              <td class="tw-whitespace-nowrap tw-px-6 tw-py-4 tw-font-medium">{{res.gete}}</td>
              <td class="tw-whitespace-nowrap tw-px-6 tw-py-4 tw-font-medium">{{res.nameGete}}</td>
              <td class="tw-whitespace-nowrap tw-px-6 tw-py-4 tw-font-medium">{{res.radif}}</td>
              <td class="tw-whitespace-nowrap tw-px-6 tw-py-4 tw-font-medium">{{res.shomare}}</td>
              <td class="tw-whitespace-nowrap tw-px-6 tw-py-4 tw-font-medium">{{res.tabage}}</td>
              <td class="tw-whitespace-nowrap tw-px-6 tw-py-4 tw-font-medium">{{res.mahalDafn}}</td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>
  </div>
</div>
          </div>
        </v-col>
      </v-row>
      <v-row>
        <v-col>
          <div class="tw-flex">
  <!-- Previous Button -->
  <a @click="goToPrevPage" class="tw-flex ml-2 tw-items-center tw-justify-center tw-px-4 tw-h-10 tw-text-base tw-font-medium tw-text-gray-500 tw-bg-white tw-border tw-border-gray-300 tw-rounded-lg tw-hover:bg-gray-100 tw-hover:text-gray-700 tw-dark:bg-gray-800 tw-dark:border-gray-700 tw-dark:text-gray-400 tw-dark:hover:bg-gray-700 tw-dark:hover:text-white">
    صفحه قبل
  </a>

  <!-- Next Button -->
  <a @click="goToNextPage" class="tw-flex tw-items-center tw-justify-center tw-px-4 tw-h-10 tw-text-base tw-font-medium tw-text-gray-500 tw-bg-white tw-border tw-border-gray-300 tw-rounded-lg tw-hover:bg-gray-100 tw-hover:text-gray-700 tw-dark:bg-gray-800 tw-dark:border-gray-700 tw-dark:text-gray-400 tw-dark:hover:bg-gray-700 tw-dark:hover:text-white">
    صفحه بعد
  </a>
</div>
        </v-col>
      </v-row>
    </v-container>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  data(){
    return {
      name: '',
      isLoading: false,
      responseData: '',
      RequiredRules: [
        v => !!v || 'این فیلد اجباری است',
        v => (v && v.length <= 40) || 'فیلد نام نمیتواند بیشتر از ۴۰ کاراکتر باشد',
      ],
      page: 1,
    }
  },

  methods:{
    search(){
      this.isLoading = true
      this.page = 1
      axios.get('http://185.172.215.158:6870/api/Search/GetSearch?FullName=' + this.name)
        .then(function (response){
          console.log(response)
          this.responseData = response.data
          this.isLoading = false
        }.bind(this))
    },
    goToNextPage(){
      this.page ++
      axios.get('http://185.172.215.158:6870/api/Search/GetSearchPagination?FullName='+ this.name + '&CurrentPage=' + this.page)
        .then(function (response){
          console.log(response)
          this.responseData = response.data
          this.isLoading = false
        }.bind(this))

    },
    goToPrevPage(){
      if(this.page == 1){
        return
      }
      this.page --
      axios.get('http://185.172.215.158:6870/api/Search/GetSearchPagination?FullName=' + this.name + '&CurrentPage=' + this.page)
        .then(function (response){
          console.log(response)
          this.responseData = response.data
          this.isLoading = false
        }.bind(this))
    }
  }

}
</script>
