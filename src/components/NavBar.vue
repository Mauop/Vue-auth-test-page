<template>
  <div class="navbar__wrapper">
    <div class="topbar">
      <div class="container">
        <div class="row middle-xs">
          <div class="col topbar__text">
            50% скидка на все программы<br class="mob" />
            при оплате онлайн до 15 декабря.<br class="mob" />
            <span>Оплати сейчас!</span>
          </div>
          <div class="col-xs-1 topbar__close">
            <div class="topbar__close__icon">
              <svg
                width="18"
                height="18"
                viewBox="0 0 18 18"
                fill="none"
                xmlns="http://www.w3.org/2000/svg"
              >
                <path d="M17 1L1 17M1 1L17 17" stroke="#FFBD87" stroke-width="2" />
              </svg>
            </div>
          </div>
        </div>
      </div>
    </div>
    <div class="navbar">
      <div class="container">
        <div class="row start-xs around-sm middle-xs">
          <div class="col-xs-12 col-sm-3 col-lg-6 p0">
            <div class="row middle-xs">
              <div class="col-xs-2 show-xs hide-sm navbar__menu__btn p0">
                <svg
                  style="margin-top:7px"
                  width="30"
                  height="20"
                  viewBox="0 0 30 20"
                  fill="none"
                  xmlns="http://www.w3.org/2000/svg"
                >
                  <rect width="30" height="4" fill="#242424" />
                  <rect y="8" width="30" height="4" fill="#242424" />
                  <rect y="16" width="30" height="4" fill="#242424" />
                </svg>
              </div>
              <div class="col-xs-6 col-sm-12 col-lg-5 navbar__header p0">
                Федеральный<br />
                Центр НМО
              </div>
              <div class="col-xs-6 hide-xs show-lg navbar__text">
                Дистанционное обучение врачей,<br />медсестер и фармацевтов
              </div>
              <div class="col-xs-4 show-xs hide-sm navbar__callanswer p0">
                <a href="tel:+78124099736">
                  <svg
                    style="float:right"
                    width="28"
                    height="28"
                    viewBox="0 0 28 28"
                    fill="none"
                    xmlns="http://www.w3.org/2000/svg"
                  >
                    <g clip-path="url(#clip0)">
                      <path
                        d="M27.3723 22.1282L23.0481 17.8035C22.1868 16.9457 20.7605 16.9718 19.8692 17.8634L17.6907 20.0414C17.553 19.9656 17.4106 19.8863 17.2608 19.8023C15.8851 19.04 14.0022 17.9952 12.0208 16.0124C10.0336 14.0254 8.98781 12.1396 8.22321 10.763C8.14253 10.6172 8.06522 10.4766 7.98888 10.343L9.45099 8.88308L10.1698 8.16337C11.0625 7.27046 11.0871 5.84455 10.2278 4.98422L5.90366 0.659097C5.04438 -0.200024 3.61742 -0.173961 2.72476 0.718945L1.50606 1.94464L1.53937 1.9777C1.13072 2.49913 0.789244 3.10051 0.535128 3.74903C0.300881 4.36635 0.15504 4.95542 0.0883533 5.54571C-0.482624 10.2792 1.68046 14.6053 7.55079 20.4757C15.6654 28.5898 22.2047 27.9768 22.4868 27.9469C23.1012 27.8734 23.69 27.7266 24.2884 27.4942C24.9313 27.2431 25.5323 26.9021 26.0534 26.4943L26.08 26.518L27.3147 25.309C28.2055 24.4163 28.2311 22.9899 27.3723 22.1282Z"
                        fill="#2f80f3"
                      />
                    </g>
                  </svg>
                </a>
              </div>
            </div>
          </div>
          <div class="col-xs-9 col-lg-6 p0 hide-xs show-sm" style="height:100%">
            <div class="row middle-xs">
              <div class="col-xs-4 navbar__phone">
                <div class="navbar__phone__text">
                  +7 (812) 409-97-36
                </div>

                <div class="navbar__phone__call">
                  <span>Заказать звонок</span>
                </div>
              </div>
              <div
                class="col-xs-offset-1 col-xs-3 navbar__login"
                @click="$emit('openmodal')"
              >
                Войти
              </div>
              <div class="col-xs-4 navbar__register">
                <button @click="$emit('openmodal')">Регистрация</button>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {}
</script>

<style lang="scss" scoped>
@import '@/assets/scss/components/_navBar.scss';
</style>
