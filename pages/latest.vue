<template>


  <div class="tw-flex tw-flex-wrap tw-w-full tw-mx-auto">

    <div v-for="latest in latestData" :key="latest.id" class="tw-basis-1/4 tw-flex-shrink mx-5 tw-w-full tw-md:w-1/2 tw-lg:w-1/2">
  <div class="tw-relative tw-overflow-x-auto tw-shadow-md tw-sm:rounded-lg">
    <table class="tw-w-full tw-text-sm tw-text-center tw-text-medium tw-text-gray-500 ">
        <tbody class="tw-max-w-md tw-min-w-sm">
            <tr class="tw-border-b tw-bg-gray-200 tw-dark:bg-gray-800 tw-dark:border-gray-700">
              <td scope="row" class="tw-px-6  tw-py-4 tw-font-medium tw-text-gray-900 tw-whitespace-nowrap tw-dark:text-white">نام</td>
              <td scope="row" class="tw-px-6 tw-py-4 tw-font-medium tw-text-gray-900 tw-whitespace-nowrap tw-dark:text-white">
                  {{ latest.fullName }}
                </td>
              </tr>
              <tr class="tw-border-b tw-bg-gray-500 tw-text-white">
                <td class="tw-px-6 tw-py-4 tw-w-5">
                  نام پدر
                </td>
                <td class="tw-px-6 tw-py-4">
                  {{ latest.father }}
                </td>
              </tr>
              <tr class="tw-border-b tw-bg-gray-200 tw-dark:bg-gray-800 tw-dark:border-gray-700">
                <td class="tw-px-6 tw-py-4">
                  تاریخ تولد
                </td>
                <td class="tw-px-6 tw-py-4">
                  {{ latest.born }}
                </td>
              </tr>
              <tr class="tw-border-b tw-bg-gray-500 tw-text-white">
                <td class="tw-px-6 tw-py-4 tw-w-5">
                  تاریخ فوت
                </td>
                <td class="tw-px-6 tw-py-4 ">
                  {{ latest.death }}
                </td>
              </tr>

              <tr class="tw-border-b tw-bg-gray-200 tw-dark:bg-gray-800 tw-dark:border-gray-700">
                <td class="tw-px-6 tw-py-4 tw-w-5">
                  فاز
                </td>
                <td class="tw-px-6 tw-py-4">
                  {{ latest.faz }}
                </td>
              </tr>

              <tr class="tw-border-b tw-bg-gray-500 tw-text-white">
                <td class="tw-px-6 tw-py-4 tw-w-5">
                  قطعه
                </td>
                <td class="tw-px-6 tw-py-4">
                  {{ latest.gete }}
                </td>
              </tr>

              <tr class="tw-border-b tw-bg-gray-200 tw-dark:bg-gray-800 tw-dark:border-gray-700">
                <td class="tw-px-6 tw-py-4 tw-w-5">
                  نام قطعه
                </td>
                <td class="tw-px-6 tw-py-4">
                  {{ latest.namegete }}
                </td>
              </tr>

              <tr class="tw-border-b tw-bg-gray-500 tw-text-white">
                <td class="tw-px-6 tw-py-4 tw-w-5">
                  ردیف
                </td>
                <td class="tw-px-6 tw-py-4">
                  {{ latest.radif }}
                </td>
              </tr>

              <tr class="tw-border-b tw-bg-gray-200 tw-dark:bg-gray-800 tw-dark:border-gray-700">
                <td class="tw-px-6 tw-py-4 tw-w-5">
                  شماره
                </td>
                <td class="tw-px-6 tw-py-4">
                  {{ latest.shomare }}
                </td>
              </tr>

              <tr class="tw-border-b tw-bg-gray-500 tw-text-white">
                <td class="tw-px-6 tw-py-4 tw-w-5">
                  طبقه
                </td>
                <td class="tw-px-6 tw-py-4">
                  {{ latest.tabage }}
                </td>
              </tr>

              <tr class="tw-border-b tw-bg-gray-200 tw-dark:bg-gray-800 tw-dark:border-gray-700">
                <td class="tw-px-6 tw-py-4 tw-w-5">
                  محل دفن
                </td>
                <td class="tw-px-6 tw-py-4">
                  {{ latest.mahaleDafn }}
                </td>
              </tr>
        </tbody>
    </table>
    <br>
</div>
</div>
</div>
</template>

<script>
import axios from 'axios'
export default {
  name: 'InspirePage',
  data(){
    return {
      latestData: '',
    }
  },
  created(){
    axios.get('http://185.172.215.158:6870/api/Search/GetCurrentDate')
      .then(function (response){
        console.log(response.data)
        this.latestData = response.data
      }.bind(this))
  }
}
</script>
