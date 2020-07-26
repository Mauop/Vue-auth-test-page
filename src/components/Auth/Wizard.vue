<template>
  <div class="wizard">
    <transition name="modal">
      <Modal v-if="authModal" @close="closeWizard">
        <template v-slot:header>
          <div class="modal__header">
            Вход или регистрация
          </div>
        </template>
        <template v-slot:body>
          <form class="modal__body__phone" @submit.prevent="onPhoneSubmit">
            <label>Номер телефона</label>
            <input
              id="phone"
              name="phone"
              type="tel"
              placeholder="+7 (___) ___-__-__"
              v-mask="'+# (###) ###-##-#####'"
              v-model="phone"
              @focus="onPhoneInput"
            />
            <button type="submit" :class="{ disabled: isDisabled }">
              Продолжить
            </button>
            <div v-if="isError" class="error">
              Для продолжения ведите корректный номер телефона
            </div>
          </form>
        </template>
        <template v-slot:footer>
          <div class="modal__footer__phone">
            Нажимая кнопку «Продолжить», я даю согласие на обработку моих персональных
            данных и принимаю настоящие <a href="">Соглашения</a>
          </div>
        </template>
      </Modal>
    </transition>
    <transition name="modal">
      <Modal v-if="smsModal" @close="closeWizard">
        <template v-slot:header>
          <div class="modal__header">
            Введите код
          </div>
        </template>
        <template v-slot:body>
          <form class="modal__body__sms" @submit.prevent="onVerifed">
            <label>Мы отправили код на {{ usedPhone }}</label>
            <div class="edit" @click="goToPhone">Изменить</div>
            <div class="code row">
              <div class="col-xs-3 p0" v-for="(i, index) in codeNumbers" :key="index">
                <input
                  :tabindex="index + 1"
                  max="9"
                  v-mask="'#'"
                  :ref="'code_' + (index + 1)"
                  type="number"
                  v-model="i.value"
                  @input="codeFocus"
                  @keyup="keyUp"
                  @keydown="keyDown"
                  @keypress="preventNaNtype"
                />
              </div>
            </div>
            <div v-if="isError" class="error">
              Вы ввели неправильный код, попробуйте еще раз
            </div>
          </form>
        </template>
        <template v-slot:footer>
          <div class="modal__footer__sms">
            <span v-if="!canNewSms" class="get__new">
              Получить новый код можно<br />
              через {{ formattedTimeLeft }}
            </span>
            <span v-else class="send__new" @click="getNewSmsCode">
              Отправить код<br class="mob" />
              еще раз
            </span>
          </div>
        </template>
      </Modal>
    </transition>
    <transition name="modal">
      <Modal v-if="verifedModal" @close="closeWizard">
        <template v-slot:header>
          <div class="modal__header__success">
            Вход выполнен
          </div>
        </template>
        <template v-slot:body>
          <div class="modal__body">
            <svg
              style="margin: 60px auto 0"
              xmlns="http://www.w3.org/2000/svg"
              viewBox="0 0 24 24"
              fill="#2f80f3"
              width="150px"
              height="150px"
            >
              <path d="M0 0h24v24H0V0z" fill="none" />
              <path d="M9 16.2L4.8 12l-1.4 1.4L9 19 21 7l-1.4-1.4L9 16.2z" />
            </svg>
          </div>
        </template>
        <template v-slot:close><span></span></template>
      </Modal>
    </transition>
  </div>
</template>

<script>
import Vue from 'vue'
import VueMask from 'v-mask'
Vue.use(VueMask)
import Modal from '@/components/Modal.vue'

export default {
  components: {
    Modal
  },
  data() {
    return {
      authModal: true,
      smsModal: false,
      verifedModal: false,
      isError: false,
      isDisabled: true,
      phone: '',
      usedPhone: '',
      codeNumbers: [
        { count: 'first', value: '' },
        { count: 'second', value: '' },
        { count: 'third', value: '' },
        { count: 'fourth', value: '' }
      ],
      rightCode: null,
      timeToResend: null,
      canNewSms: false,
      timePassed: 0,
      timerInterval: null
    }
  },
  computed: {
    smsCodeFull() {
      const code1 = this.codeNumbers[0].value.charAt(0)
      const code2 = this.codeNumbers[1].value.charAt(0)
      const code3 = this.codeNumbers[2].value.charAt(0)
      const code4 = this.codeNumbers[3].value.charAt(0)

      return code1 + code2 + code3 + code4 || ''
    },
    formattedTimeLeft() {
      const timeLeft = this.timeLeft
      const minutes = Math.floor(timeLeft / 60)
      const seconds = timeLeft % 60 < 10 ? `0${timeLeft % 60}` : timeLeft % 60

      return `${minutes}:${seconds}`
    },
    timeLeft() {
      return this.timeToResend - this.timePassed
    }
  },
  mounted() {
    this.$nextTick(function() {
      if (this.authModal && window.matchMedia('(max-width: 990px)').matches) {
        document.getElementById('phone').focus()
      }
    })
  },
  methods: {
    closeWizard() {
      this.authModal = this.smsModal = this.verifedModal = false
      this.$emit('close')
    },
    onPhoneSubmit() {
      if (this.isDisabled) {
        this.isError = true
      } else {
        this.isError = false
        this.usedPhone = this.phone
        this.goToCode()
      }
    },
    goToPhone() {
      this.phone = this.usedPhone
      this.smsModal = false
      this.isError = false
      this.codeNumbers.forEach(item => {
        item.value = ''
      })
      this.authModal = true
      setTimeout(() => {
        document.getElementById('phone').focus()
      }, 0)
    },
    async goToCode() {
      this.authModal = false
      this.smsModal = true
      await this.fetchCode()
      this.$refs.code_1[0].focus()
    },
    onPhoneInput() {
      if (this.phone.length < 3) {
        this.phone = '+7'
      }
    },
    onVerifed() {
      this.smsModal = false
      this.isError = false
      this.codeNumbers.forEach(item => {
        item.value = ''
      })
      this.verifedModal = true
      setTimeout(() => {
        this.closeWizard()
      }, 1500)
    },

    /* Fake API */

    async fetchCode() {
      if (!this.timerInterval) {
        try {
          await fetch(
            'https://my-json-server.typicode.com/mauop/vue-auth-test-page/blob/master/code'
          )
            .then(res => {
              return res.json()
            })
            .then(data => {
              if (!this.timerInterval) {
                this.timerInterval = setInterval(() => this.timePassed++, 1000)
              }
              this.rightCode = data.value
              this.timeToResend = parseInt(data.timeToResend) || 60
              this.canNewSms = false
            })
        } catch (e) {
          console.log(e)
        }
      }
    },

    async checkSmsCode() {
      if (!this.rightCode) {
        await this.fetchCode()
      }

      if (this.rightCode === this.smsCodeFull) {
        this.isError = false
        this.onVerifed()
      } else {
        this.isError = true
      }
    },

    getNewSmsCode() {
      this.fetchCode()
    },

    onTimesUp() {
      clearInterval(this.timerInterval)
      this.timerInterval = null
      this.timePassed = 0
    },

    /* Fake API */

    /* SMS CODE INPUTS EVENTS */

    preventNaNtype(event) {
      if (event.keyCode < 48 || event.keyCode > 57) {
        event.preventDefault()
      }
    },

    keyDown(e) {
      if (e.keyCode === 8 || e.keyCode === 38 || e.keyCode === 40 || e.keyCode === 46) {
        e.preventDefault()
      }
    },

    keyUp(e) {
      const curTabIndex = parseInt(e.target.tabIndex)
      const prevTabIndex = curTabIndex - 1 || 1
      const nextTabIndex = curTabIndex + 1 || 4

      switch (e.keyCode) {
        case 37: //arrow left
          if (curTabIndex > 1) {
            this.$refs['code_' + prevTabIndex][0].focus()
          }
          break
        case 39: //arrow right
          if (curTabIndex < 4) {
            this.$refs['code_' + nextTabIndex][0].focus()
          }
          break
        case 46: //delete
          if (e.target.value.length === 0 && curTabIndex > 1) {
            this.codeNumbers[prevTabIndex - 1].value = ''
            this.$refs['code_' + prevTabIndex][0].focus()
          } else if (e.target.value.length !== 0) {
            this.codeNumbers[curTabIndex - 1].value = ''
          }
          break
        case 8: //backspace
          if (e.target.value.length === 0 && curTabIndex > 1) {
            this.$refs['code_' + prevTabIndex][0].focus()
            this.codeNumbers[prevTabIndex - 1].value = ''
          } else if (e.target.value.length !== 0) {
            this.codeNumbers[curTabIndex - 1].value = ''
          }
          break
        default:
          break
      }
    },

    codeFocus(e) {
      const len = e.target.value.length
      const curTabIndex = parseInt(e.target.tabIndex)
      const nextTabIndex = curTabIndex + 1
      const prevTabIndex = curTabIndex - 1 || 1
      if (len === 1 && curTabIndex < 4) {
        this.$refs['code_' + nextTabIndex][0].focus()
      } else if (len === 0 && prevTabIndex !== 0) {
        this.$refs['code_' + prevTabIndex][0].focus()
      } else if (len > 1 && curTabIndex < 4) {
        this.codeNumbers[curTabIndex].value = this.$refs[
          'code_' + curTabIndex
        ][0].value.charAt(1)
      }
    }

    /* SMS CODE INPUTS EVENT S */
  },
  watch: {
    phone(val) {
      if (val.length >= 18) {
        this.isDisabled = this.isError = false
      } else {
        this.isDisabled = true
      }
    },
    timeLeft(newValue) {
      if (newValue === 0) {
        this.onTimesUp()
        this.canNewSms = true
      }
    },
    smsCodeFull(val) {
      if (val.length === 4) {
        this.checkSmsCode()
      }
    }
  }
}
</script>

<style lang="scss" scoped>
@import '@/assets/scss/components/_modal.scss';
</style>
