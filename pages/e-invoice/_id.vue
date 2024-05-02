<template>
  <div class="static_page_content_wrapper invoice">
    <MainLoader v-if="isLoading" />

    <div class="container">
      <div class="page_title">
        <h2>{{ $t('user.invoice_data') }}</h2>
      </div>

      <form action="" v-if="!show_hint">

        <div class="row">
          <div class="col-12">
            <div class="image">
              <img :src="qrImage" alt="">
            </div>
          </div>

          <div class="col-lg-3 col-12">
            <div class="form-group">
              <label for="">{{ $t('user.invoice_num') }}</label>
              <input type="text" readonly v-model="invoice_num" />
            </div>
          </div>

          <div class="col-lg-3 col-12">
            <div class="form-group">
              <label for="">{{ $t('user.invoice_date') }}</label>
              <input type="text" readonly v-model="invoice_date" />
            </div>
          </div>

          <div class="col-lg-3 col-12">
            <div class="form-group">
              <label for="">{{ $t('user.client_name') }}</label>
              <input type="text" readonly v-model="client_name" />
            </div>
          </div>

          <div class="col-lg-3 col-12">
            <div class="form-group">
              <label for="">{{ $t('invoice.service_provider_name') }} </label>
              <input type="text" readonly v-model="service_provider_name" />
            </div>
          </div>

          <div class="col-lg-3 col-12">
            <div class="form-group">
              <label for="">{{ $t('invoice.job_title') }} </label>
              <input type="text" readonly v-model="job_title" />
            </div>
          </div>

          <div class="col-lg-3 col-12">
            <div class="form-group">
              <label for="">{{ $t('invoice.specialization') }} </label>
              <input type="text" readonly v-model="specialization" />
            </div>
          </div>

          <div class="col-lg-3 col-12">
            <div class="form-group">
              <label for="">{{ $t('invoice.years_of_experience') }} </label>
              <input type="text" readonly v-model="years_of_experience" />
            </div>
          </div>

          <div class="col-lg-3 col-12">
            <div class="form-group">
              <label for="">{{ $t('invoice.workplace') }} </label>
              <input type="text" readonly v-model="workplace" />
            </div>
          </div>
          <div class="col-lg-3 col-12">
            <div class="form-group">
              <label for="">{{ $t('invoice.address') }} </label>
              <input type="text" readonly v-model="address" />
            </div>
          </div>
          <div class="col-lg-3 col-12">
            <div class="form-group">
              <label for="">{{ $t('invoice.work_dates') }} </label>
              <input type="text" readonly v-model="work_dates" />
            </div>
          </div>

          <div class="col-lg-3 col-12">
            <div class="form-group">
              <label for="">{{ $t('invoice.hours_per_day') }} </label>
              <input type="text" readonly v-model="hours_per_day" />
            </div>
          </div>

          <div class="col-lg-3 col-12">
            <div class="form-group">
              <label for="">{{ $t('invoice.total_working_hours') }} </label>
              <input type="text" readonly v-model="total_working_hours" />
            </div>
          </div>

          <div class="col-lg-3 col-12">
            <div class="form-group">
              <label for="">{{ $t('invoice.hourly_rate') }} </label>
              <input type="text" readonly v-model="hourly_rate" />
            </div>
          </div>

          <div class="col-lg-3 col-12">
            <div class="form-group">
              <label for="">{{ $t('invoice.total_job_payment_without_tax') }} </label>
              <input type="text" readonly v-model="total_job_payment_without_tax" />
            </div>
          </div>
          <div class="col-lg-3 col-12">
            <div class="form-group">
              <label for="">{{ $t('invoice.paid_amount_to_employee_without_tax') }} </label>
              <input type="text" readonly v-model="paid_amount_to_employee_without_tax" />
            </div>
          </div>
          <div class="col-lg-3 col-12">
            <div class="form-group">
              <label for="">{{ $t('invoice.vat') }} </label>
              <input type="text" readonly v-model="vat" />
            </div>
          </div>
          <div class="col-lg-3 col-12">
            <div class="form-group">
              <label for="">{{ $t('invoice.total_delivery_price') }} </label>
              <input type="text" readonly v-model="total_delivery_price" />
            </div>
          </div>

        </div>

      </form>

      <div class="page_title" v-else>
        <h2 style="color:red;font-weight:bold">{{ hint }}</h2>
      </div>

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

      invoice_num: null,
      qrImage: null,
      invoice_date: null,
      driver_name: null,
      provider_name: null,
      main_shipment_type: null,
      sub_shipment_type: null,
      shipment_length: null,
      shipment_width: null,
      shipment_height: null,
      shipment_weight: null,
      shipment_description: null,
      license_plate_number: null,
      delivery_request_price: null,
      discount_code: null,
      discounted_delivery_price: null,
      vat: null,
      total_delivery_price: null,


      code: '',
      target: ''
    };


  },

  methods: {
    async getData() {

      this.code = this.$route.query.code || '';
      this.target = this.$route.query.target || '';

      await this.$axios({
        url: `api/v1/core/e-invoice?code=${this.code}&target=${this.target}`,
        method: "GET",
        headers: {
          "Accept-language": this.$nuxt.$i18n.locale,
          lang: this.$nuxt.$i18n.locale,
        },
      }).then(response => {
        console.log(response);

        this.invoice_num = response.data.data.id;
        this.qrImage = response.data.data.qrImage;
        this.invoice_date = response.data.data.created_at;
        this.driver_name = response.data.data.provider_name;
        this.provider_name = response.data.data.client_name;
        this.main_shipment_type = response.data.data.shipment_type;
        this.sub_shipment_type = response.data.data.shipment_sub_type;
        this.shipment_length = response.data.data.length;
        this.shipment_width = response.data.data.width;
        this.shipment_height = response.data.data.height;
        this.shipment_weight = response.data.data.weight;
        this.shipment_description = response.data.data.description;
        this.license_plate_number = response.data.data.board_number;
        this.delivery_request_price = response.data.data.price;
        this.discount_code = response.data.data.coupon_code;
        this.vat = response.data.data.vat;
        this.discounted_delivery_price = response.data.data.price_discount;
        this.total_delivery_price = response.data.data.final_price;


      }).catch(error => {
        this.show_hint = true;
        this.hint = error.response.data.message;
        console.log(error.response.data.message);
      });

    }
  },


  mounted() {

    this.getData();

    setTimeout(() => {
      this.isLoading = false;
      document.body.style.overflow = "unset";
    }, 1000);
  },
};
</script>

<style lang="scss" scoped>
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

.invoice {
  margin: 70px 0 30px;

  form {

    .form-group {
      // margin: 10px 0;
    }

    .image {
      text-align: center;

      img {
        width: 200px;
        height: 200px;
        border-radius: 10px;
        object-fit: contain;
      }
    }

    input {
      width: 100%;
      height: 45px;
      border: 1px solid #18539c;
      border-radius: 8px;
      margin-top: 10px;
      outline: none;
      text-indent: 20px;
      margin-bottom: 10px;

      &:focus-visible {
        border: 1px solid #18539c !important;
      }
    }
  }


}



.static_page_content_wrapper {
  padding-block: 30px 40px;
  background-color: #f8f8f8;

  .page_title {
    h2 {
      margin-bottom: 20px;
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
