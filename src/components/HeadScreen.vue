<template>
  <div class="head__screen">
    <div class="middle head__img_bg">
      <div class="container">
        <div class="row middle-xs">
          <div class="col-xs-12 col-lg-8 cta__col">
            <div class="main__header">
              Зарегистрируйся<br class="mob" />
              сейчас<br class="hide-xs-only" />
              и получи бесплатно 14 ЗЕТ (баллов НМО)
            </div>
            <div class="row middle-xs form">
              <div class="col-xs-12 col-sm-7 col-lg-5 p0">
                <input
                  placeholder="+7 (___)___-__-__"
                  type="tel"
                  v-mask="'+# (###) ###-##-#####'"
                  v-model="phone"
                  @focus="onPhoneInput"
                />
              </div>
              <div class="col-xs-12 col-sm-5 p0">
                <button @click="$emit('openmodal')">Зарегистрироваться</button>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
    <div class="footer">
      <div class="container">
        <div class="footer__row row middle-xs">
          <div class="col-xs-12 col-sm-4 col-lg-3 p0">
            <div class="row middle-xs">
              <div class="col-xs-2 f__icon">
                <img src="../assets/icons/doctor 1.svg" alt="Практикующие врачи" />
              </div>
              <div class="col-xs-10 mob__main">
                <div class="text">
                  Созданы ведущими<br />
                  экспертами<br />
                  и практикующими врачами
                </div>
              </div>
              <div class="col-xs-2 hide-sm">
                <svg
                  width="15"
                  height="24"
                  viewBox="0 0 15 24"
                  fill="none"
                  xmlns="http://www.w3.org/2000/svg"
                >
                  <path d="M2 2L12 12L2 22" stroke="white" stroke-width="3" />
                </svg>
              </div>
            </div>
          </div>
          <div class="col-xs-4 col-lg-3 p0 hide-xs show-sm">
            <div class="row middle-xs end-xs">
              <div class="col-xs-2 f__icon">
                <img src="../assets/icons/clock 1.svg" alt="Дистанционный прием" />
              </div>
              <div class="col-xs-8">
                <div class="text">
                  Дистанционно<br />
                  в удобное<br />
                  для Вас время
                </div>
              </div>
            </div>
          </div>
          <div class="col-xs-4 col-lg-3 p0 hide-xs show-sm">
            <div class="row middle-xs end-xs">
              <div class="col-xs-2 f__icon">
                <img
                  src="../assets/icons/stethoscope 1.svg"
                  alt="Доказательная медицина"
                />
              </div>
              <div class="col-xs-8">
                <div class="text">
                  Только с позиции<br />
                  доказательной<br />
                  медицины
                </div>
              </div>
            </div>
          </div>
          <div class="col-xs-3 p0 hide-xs show-lg">
            <div class="row middle-xs end-xs">
              <div class="col-xs-2 f__icon">
                <img
                  src="../assets/icons/document 1.svg"
                  alt="Современные международные руководства"
                />
              </div>
              <div class="col-xs-10">
                <div class="text">
                  Опираемся на самые<br />
                  современные международные<br />
                  руководства
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import Vue from 'vue'
import VueMask from 'v-mask'
Vue.use(VueMask)

export default {
  data() {
    return {
      phone: ''
    }
  },

  methods: {
    onPhoneInput() {
      if (this.phone.length < 3) {
        this.phone = '+7'
      }
    }
  }
}
</script>

<style lang="scss" scoped>
@import '@/assets/scss/components/_headScreen.scss';
</style>
