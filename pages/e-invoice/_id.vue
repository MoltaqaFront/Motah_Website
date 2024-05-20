<template>
  <div class="static_page_content_wrapper invoice">
    <!-- <MainLoader v-if="isLoading" /> -->

    <div class="container">
      <div class="page_title">
        <h2>{{ $t('user.invoice_data') }}</h2>
      </div>

      <!--  v-if="!show_hint" -->
      <form action="">

        <div class="row justify-content-center">

          <div class="col-lg-8 col-12">
            <!-- <div class="col-12">
            <div class="image">
              <img :src="qrImage" alt="">
            </div>
          </div> -->


            <!-- <div class="section_info"> -->

            <div class="row">

              <div class="col-12">
                <div class="form-group">
                  <span>{{ $t('user.invoice_num') }}</span>
                  <span>{{ invoice_num }}</span>
                </div>
              </div>

              <div class="col-12">

                <div class="form-group">
                  <span>{{ $t('user.invoice_date') }}</span>
                  <span>{{ invoice_date }}</span>
                </div>

              </div>

            </div>

            <!-- </div> -->


            <div class="row">

              <div class="col-12">
                <div class="form-group">
                  <span>{{ $t('invoice.client_name') }}</span>
                  <span>{{ client_name }}</span>
                </div>
              </div>

              <div class="col-12">

                <div class="form-group">
                  <span>{{ $t('invoice.service_provider_name') }}</span>
                  <span>{{ service_provider_name }}</span>
                </div>

              </div>

            </div>

            <div class="row">

              <div class="col-12">
                <div class="form-group">
                  <span>{{ $t('invoice.job_title') }}</span>
                  <span>{{ job_title }}</span>
                </div>
              </div>

              <div class="col-12">

                <div class="form-group">
                  <span>{{ $t('invoice.specialization') }}</span>
                  <span>{{ specialization }}</span>
                </div>

              </div>

            </div>

            <div class="row">

              <div class="col-12">
                <div class="form-group">
                  <span>{{ $t('invoice.years_of_experience') }}</span>
                  <span>{{ years_of_experience }}</span>
                </div>
              </div>

              <div class="col-12">

                <div class="form-group">
                  <span>{{ $t('invoice.workplace') }}</span>
                  <span v-if="workplace == 'remote'">{{ $t('invoice.remote') }}</span>
                  <span v-else>{{ $t('invoice.home') }}</span>
                </div>

              </div>

            </div>

            <div class="row">

              <div class="col-12" v-if="workplace !== 'remote'">
                <div class="form-group">
                  <span>{{ $t('invoice.address') }}</span>
                  <span>{{ address }}</span>
                </div>
              </div>

              <div class="col-12">

                <div class="form-group">
                  <span>{{ $t('invoice.work_dates') }}</span>
                  <span>{{ work_dates }}</span>
                </div>

              </div>

            </div>

            <div class="row">

              <div class="col-12">
                <div class="form-group">
                  <span>{{ $t('invoice.hours_per_day') }}</span>
                  <span>{{ hours_per_day }}</span>
                </div>
              </div>

              <div class="col-12">

                <div class="form-group">
                  <span>{{ $t('invoice.total_working_hours') }}</span>
                  <span>{{ total_working_hours }}</span>
                </div>

              </div>

            </div>

            <div class="row">

              <div class="col-12">
                <div class="form-group">
                  <span>{{ $t('invoice.hourly_rate') }}</span>
                  <span>{{ hourly_rate }}</span>
                </div>
              </div>

              <div class="col-12">

                <div class="form-group">
                  <span>{{ $t('invoice.total_job_payment_without_tax') }}</span>
                  <span>{{ total_job_payment_without_tax }}</span>
                </div>

              </div>

            </div>

            <div class="row">

              <div class="col-12">
                <div class="form-group">
                  <span>{{ $t('invoice.paid_amount_to_employee_without_tax') }}</span>
                  <span>{{ paid_amount_to_employee_without_tax }}</span>
                </div>
              </div>

              <div class="col-12">

                <div class="form-group">
                  <span>{{ $t('invoice.vat') }}</span>
                  <span>{{ vat }}</span>
                </div>

              </div>

            </div>

            <div class="row">

              <div class="col-12">
                <div class="form-group">
                  <span>{{ $t('invoice.total_delivery_price') }}</span>
                  <span>{{ total_delivery_price }}</span>
                </div>
              </div>

              <div class="col-12">

                <div class="form-group">
                  <span>{{ $t('invoice.app_comm') }}</span>
                  <span>{{ app_commision }}</span>
                </div>

              </div>

            </div>

          </div>

        </div>


      </form>

      <!-- v-else -->
      <!-- <div class="page_title">
        <h2 style="color:red;font-weight:bold">{{ hint }}</h2>
      </div> -->

    </div>
  </div>
</template>

<script>
import MainLoader from "~/components/ui/MainLoader.vue";
export default {
  name: "TermsAndConditions",

  layout: "staticContent",

  components: {
    MainLoader,
  },

  data() {
    return {
      isLoading: true,

      hint: "",

      show_hint: false,

      qrImage: null,
      invoice_num: null,
      invoice_date: null,
      client_name: null,
      service_provider_name: null,
      job_title: null,
      specialization: null,
      years_of_experience: null,
      workplace: null,
      address: null,
      work_dates: null,
      hours_per_day: null,
      total_working_hours: null,
      hourly_rate: null,
      total_job_payment_without_tax: null,
      paid_amount_to_employee_without_tax: null,
      vat: null,
      total_delivery_price: null,
      app_commision: null,


      code: '',
      target: ''
    };


  },

  methods: {
    async getData() {

      // http://localhost:3000/e-invoice?job_id=MzQy&target=YWRtaW4%3D

      this.code = this.$route.query.job_id || '';
      this.target = this.$route.query.target || '';


      await this.$axios({
        url: `dashboard/jobs/qr-job?job_id=${this.code}&target=${this.target}`,
        method: "GET",
        headers: {
          "Accept-language": this.$nuxt.$i18n.locale,
          lang: this.$nuxt.$i18n.locale,
        },
      }).then(response => {
        console.log(response);

        this.invoice_num = response.data.data.job_number;
        this.qrImage = response.data.data.qr_code;
        this.invoice_date = response.data.data.created_at;
        this.client_name = response.data.data.employee.name;
        this.service_provider_name = response.data.data.provider.name;
        this.job_title = response.data.data.info.job_title.name;
        this.specialization = response.data.data.info.job_title.specialist.name;
        this.years_of_experience = response.data.data.info.experience;
        this.workplace = response.data.data.info.work_space;
        this.address = response.data.data.info.location;
        this.work_dates = response.data.data.info.from_date + '--->' + response.data.data.info.to_date;
        this.hours_per_day = response.data.data.info.daily_hour;
        this.total_working_hours = response.data.data.info.total_working_hour;
        this.hourly_rate = response.data.data.budgets.salary_per_hour;
        this.vat = response.data.data.budgets.tax;
        this.app_commision = response.data.data.budgets.app_commission;
        this.total_delivery_price = response.data.data.budgets.final_salary;

        this.total_job_payment_without_tax = response.data.data.budgets.total_without_tax;
        this.paid_amount_to_employee_without_tax = response.data.data.budgets.employee_salary;


      }).catch(error => {
        this.show_hint = true;
        this.hint = error.response.data.message;
        console.log(error.response.data.message);
      });

    }
  },


  mounted() {

    this.getData();

    // setTimeout(() => {
    //   this.isLoading = false;
    //   document.body.style.overflow = "unset";
    // }, 1000);
  },
};
</script>

<style lang="scss" scoped>
.section_info {
  // margin-bottom: 20px;
  // padding-bottom: 20px;
  // border-bottom: 1px solid #DDD;


}

.form-group {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 10px;

  span {
    font-size: 18px;
    font-weight: 600
  }


}

.responsive_table {
  overflow-x: auto;
  overflow-y: hidden;
}

table {
  min-width: 700px;
  margin: 30px 0;

  thead {
    tr {
      background: #18539c;
      color: #FFF
    }
  }

  tbody {
    tr {
      background: #EEE;
    }
  }

  tfoot {
    tr {
      background: #18539c;

      color: #FFF;

      td {
        text-align: center;
      }
    }
  }

  tr {

    td,
    th {
      text-align: center;

    }
  }
}

// .invoice {
//   margin: 40px 0 30px;

//   form {

//     .form-group {
//       // margin: 10px 0;
//     }

//     .image {
//       text-align: center;

//       img {
//         width: 200px;
//         height: 200px;
//         border-radius: 10px;
//         object-fit: contain;
//       }
//     }

//     input {
//       width: 100%;
//       height: 45px;
//       border: 1px solid #18539c;
//       border-radius: 8px;
//       margin-top: 10px;
//       outline: none;
//       text-indent: 20px;
//       margin-bottom: 10px;

//       &:focus-visible {
//         border: 1px solid #18539c !important;
//       }
//     }
//   }


// }



.static_page_content_wrapper {
  padding-block: 30px 40px;
  background-color: #f8f8f8;

  .page_title {
    h2 {
      margin-bottom: 50px;
      text-align: center;
      font-size: 35px;
      color: var(--main_theme_clr);
    }
  }

  .page_content {
    margin-top: 25px;

    .item_title,
    .item_content {
      word-break: break-word;
      word-spacing: 2px;
      line-height: 1.6;
      text-align: start;
    }

    .item_title {
      font-size: 22px;
      color: var(--main_theme_clr);
    }

    .item_content {
      font-size: 17px;
      color: var(--light_gray_clr);
    }
  }
}
</style>
