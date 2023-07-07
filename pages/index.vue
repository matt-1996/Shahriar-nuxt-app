<template>
  <div >
    <!-- <div v-if="responseMessage" >
    </div> -->
    <div class="w-full mx-5" >
      <v-form enctype=multipart/form-data>
      <v-combobox
      label="آرامگاه"
      :rules="RequiredRules"
      :items="Cemetries"
      item-text="name"
      v-model="Cemetery_ID"
      @change="ShowSelectedCemetry">
      </v-combobox>
      <v-combobox
      label="جنسیت"
      :rules="RequiredRules"
      :items="gender"
      item-text="name"
      v-model="Gender_ID"
      @change="ShowSelectedGender">
      </v-combobox>
      <v-text-field label="نام" v-model="name" required :rules="RequiredRules"></v-text-field>
      <v-text-field label="نام خانوادگی" v-model="lastName" required :rules="RequiredRules"></v-text-field>
      <v-text-field label="نام پدر" v-model="fatherName" required :rules="RequiredRules"></v-text-field>
      <v-text-field label="کد ملی" v-model="NationalCode" required :rules="RequiredRules"></v-text-field>
      <v-text-field label="تعداد حفره ای یا تعداد قبر که شخص میخواد" v-model="NumberHolesRequested" required :rules="RequiredRules"></v-text-field>
      <v-text-field label="موبایل" v-model="Mobile" required :rules="RequiredRules"></v-text-field>
      <!-- <input type="file" id="file" :v-model="NationalCard" @change="onAddMeliCard"/> -->
      <v-file-input
      label="آپلود کارت ملی"
      density="compact"
      id="file"
      accept="image/png, image/jpeg, image/jpg"
      :v-model="NationalCard"
      @change="onAddMeliCard"
      required

      :rules="RequiredRules"></v-file-input>
      <v-file-input
      id="birthCertificate"
      label="آپلود شناسنامه"
      @change="onAddBirthCertificate"
      density="compact"
      accept="image/png, image/jpeg, image/jpg"
      v-model="BirthCertificate"
      required
      :rules="RequiredRules"
      ></v-file-input>
      <v-file-input
      label="آپلود سند خانه"
      density="compact"
      accept="image/png, image/jpeg, image/jpg"
      v-model="IdentityOfResidence"
      @change="onAddIdentityOfResidence"
      id="IdentityOfResidence"
      required
      :rules="RequiredRules"></v-file-input>
      </v-form>
      <div class="w-full">
        <v-btn block @click="sendData" :disabled="isLoading == true" type="submit">ثبت اطلاعات</v-btn>
      </div>
    </div>
  </div>
</template>
<script>
import axios from 'axios'
export default {
  name: 'IndexPage',
  data () {
    return {
      SelectedCemetry: '',
      selectedGender: '',
      Cemetery_ID: '',
      Gender_ID:'',
      isLoading: false,
      // Cemetries: {
      //   'names' : ['بهشت رضا امیریه' , 'بهشت رضوان شهریار'],
      //   'first' : {'id': 1 , 'name': 'بهشت رضا امیریه', 'id': 5 , 'name': 'بهشت رضوان شهریار'},
      //   // 'second' : {'id': 5 , 'name' : 'بهشت رضوان شهریار'}
      //   },
      Cemetries: [
          {id: 1 , name: 'بهشت رضا امیریه'} ,
          {id: 5 , name:'بهشت رضوان شهریار'}
        ],
        gender: [
          {id: 1 , name: 'مرد'},
          {id: 2 , name: 'زن'},
        ],
        RequiredRules: [
        v => !!v || 'این فیلد اجباری است',
        // v => (v && v.length <= 10) || 'Name must be less than 10 characters',
      ],
      name: '',
      lastName: '',
      NationalCode: '',
      fatherName: '',
      NumberHolesRequested: '',
      Mobile: '',
      NationalCard: '',
      BirthCertificate: '',
      IdentityOfResidence: '',
      responseMessage: '',
    }
  },
  mounted () {
    //console.log(this.items.name)

  },
  created () {
    // axios.get('https://iknowcenter.ir/wp-json/wp/v2/posts').then((response) => {
    //   this.posts = response.data
    // })
  },
  methods: {
    onAddMeliCard(event){
      var fs = require('fs');
      var MeliCardFile = document.querySelector('#file');
      this.NationalCard = MeliCardFile.files[0];
      // fs.createReadStream('file')
      // console.log(imagefile.files[0])
    },
    onAddBirthCertificate(event){
      var fs = require('fs');
      var MeliCardFile = document.querySelector('#birthCertificate');
      this.NationalCard = MeliCardFile.files[0];
      // fs.createReadStream('file')
      // console.log(imagefile.files[0])
    },
    onAddIdentityOfResidence(event){
      var fs = require('fs');
      var MeliCardFile = document.querySelector('#IdentityOfResidence');
      this.NationalCard = MeliCardFile.files[0];
      // fs.createReadStream('file')
      // console.log(imagefile.files[0])
    },
    onFileUpload(event){
      //this.NationalCard = event.target.files[0]
      console.log(this.NationalCard)
    },
    sendData () {
     //console.log({cemetry: this.SelectedCemetry.id, gender: this.selectedGender.id, name: this.name,lastName: this.lastName,fatherName: this.fatherName, idCode: this.id})
    //  let formData = new FormData();
     var FormData = require('form-data');
     var fs = require('fs');
     var data = new FormData();
     this.isLoading = true
     data.append('Cemetery_ID', this.Cemetery_ID.id);
     data.append('Gender_ID', this.Gender_ID.id);
     data.append('FirstName', this.name);
     data.append('LastName', this.lastName);
     data.append('FatherName', this.fatherName);
     data.append('NationalCode', this.NationalCode);
     data.append('NumberHolesRequested', this.NumberHolesRequested);
     data.append('Mobile', this.Mobile);
     data.append('NationalCard', this.NationalCard);
     data.append('BirthCertificate', this.BirthCertificate);
     data.append('IdentityOfResidence', this.IdentityOfResidence);

     var config = {
  method: 'post',
  url: 'http://185.172.215.158:6870/api/PresaleRequest/Insert',
  headers: {

  },
  data : data
};

axios(config)
.then(function (response) {
  console.log(JSON.stringify(response.data.value.message));
  this.isLoading = false
  var status = JSON.stringify(response.data.value.message)
  var statusCode = JSON.stringify(response.data.value.code)
  var stringMessage = status.replace(/['"]+/g, '')
  console.log(stringMessage)
    this.$swal.fire(
      'پیغام',
      stringMessage,
      'info',
    );

}.bind(this))
.catch(function (error) {
  this.isLoading = false
  console.log(error);
  this.$swal.fire({
  icon: 'error',
  title: 'خطا',
  text: 'خطایی رخ داد دوباره امتحان کنید'
})
}.bind(this));


    // let formNationalCard = formData.append('NationalCard', this.NationalCard);
    // let formBirthCertificate = formData.append('BirthCertificate', this.BirthCertificate);
    // let formIdentityOfResidence = formData.append('IdentityOfResidence', this.IdentityOfResidence);
    //  let Data = {Cemetery_ID: this.Cemetery_ID.id, Gender_ID: this.Gender_ID.id, name:this.name,
    //             lastName: this.lastName, fatherName: this.fatherName, Mobile:this.Mobile,
    //             NationalCode:this.NationalCode, NumberHolesRequested: this.NumberHolesRequested,
    //             //NationalCard: formNationalCard, BirthCertificate:formBirthCertificate, IdentityOfResidence:formIdentityOfResidence
    //            formData
    //           }

      //           console.log(Data)
      // axios.post('185.172.215.158:6870/api/PresaleRequest/Insert',Data).then((response) => {
      //       console.log(response.data)
      //   })
      //   .catch((e) => {
      //       console.log('error')
      //   })
    },
    ShowSelectedCemetry(){
      console.log(this.Cemetery_ID.id,)
    },
    ShowSelectedGender(){
      console.log(this.Gender_ID.id)
    }
  }
}
</script>
<style scoped>
</style>
