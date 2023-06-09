<script setup>
import { reactive } from "vue";

const props = defineProps({
  imageUri: {
    type: String,
    default: "../../../../assets/no-image-available.png",
  },
  width: {
    type: Number,
    default: 515,
  },
  height: {
    type: Number,
    default: 715,
  },
  title: {
    type: String,
    required: true,
  },
  description: {
    type: String,
    required: true,
  },
  stars: {
    type: Number,
    default: 0,
  },
  ranking: {
    type: String,
    default: "###",
  },
  originalPrice: {
    type: Number,
    required: true,
  },
  discountPrice: {
    type: Number,
    required: true,
  },
  availableRooms: {
    type: Number,
    required: true,
  },
  stayDate: {
    type: String,
    required: true,
  },
  wifiIcon: { type: Boolean, default: false },
  foodIcon: { type: Boolean, default: false },
  swimIcon: { type: Boolean, default: false },
});

const state = reactive({
  isFavorite: false,
  bookingRoom: false,
});

const iconsPath = "./src/components/HotelCard/assets/icons";
const favoriteHotelIcon = `${iconsPath}/filled-heart.svg`;
const noFavoriteHotelIcon = `${iconsPath}/heart.svg`;

const calcSavedValue = () => {
  return props.originalPrice - props.discountPrice;
};

const getImgUrl = (imageUri) => {
  return "./src/components/HotelCard/assets/img/" + imageUri;
};

const setAsFavorite = () => {
  state.isFavorite = !state.isFavorite;
};

const initBookingRoom = () => {
  state.bookingRoom = true;
};

const closeBookingArea = () => {
  state.bookingRoom = false;
};
</script>

<template>
  <div
    :style="{ width: `${width}px`, height: `${height}px` }"
    class="hotel-card"
  >
    <div v-if="!state.bookingRoom" class="display-flex-column">
      <div class="hotel-card__image-container">
        <div class="hotel-card__info-wrapper" @click="setAsFavorite">
          <img
            v-if="state.isFavorite"
            class="hotel-card__info-wrapper--fav-icon"
            :src="favoriteHotelIcon"
            alt="Favorite icon"
          />
          <img
            v-else
            class="hotel-card__info-wrapper--fav-icon"
            :src="noFavoriteHotelIcon"
            alt="Favorite icon"
          />
        </div>
        <img
          :src="getImgUrl(imageUri)"
          alt="hotel image"
          class="hotel-card__image"
        />
      </div>
      <div class="hotel-card__body-container">
        <div class="hotel-card__title-wrapper">
          <div class="hotel-card__title-wrapper--title">{{ title }}</div>
          <div class="hotel-card__title-wrapper__icons-container">
            <div v-if="swimIcon" class="icon-default" title="Pool">
              <img src="./assets/icons/swim.svg" alt="Swim indicator" />
            </div>
            <div v-if="wifiIcon" class="icon-default" title="Wifi">
              <img src="./assets/icons/wifi.svg" alt="Wifi indicator" />
            </div>
            <div v-if="foodIcon" class="icon-default" title="Food">
              <img src="./assets/icons/food.svg" alt="Food indicator" />
            </div>
          </div>
        </div>
        <div class="hotel-card__infos-wrapper">
          <div class="hotel-card__left-infos-wrapper">
            <div class="flex-row">
              <img
                v-for="(star, index) in 5"
                :key="index"
                :src="`${
                  index < stars
                    ? './src/components/HotelCard/assets/icons/filled-star.svg'
                    : './src/components/HotelCard/assets/icons/star.svg'
                }`"
                alt="Stars indicator"
              />
              <div class="hotel-card__left-infos-wrapper--ranking">
                {{ ranking }}
              </div>
            </div>
            <div class="hotel-card__left-infos-wrapper--secondary">
              <div class="hotel-card--label">{{ stayDate }}</div>
              <div class="hotel-card--label">{{ availableRooms }}</div>
            </div>
          </div>
          <div class="hotel-card__right-infos-wrapper">
            <div class="hotel-card__right-infos-wrapper--discount-price">
              ${{ discountPrice }}<span class="fs-18">/night</span>
            </div>
            <div class="hotel-card__right-infos-wrapper--original-price">
              <span class="line-through">${{ originalPrice }}</span>
              <span class="fs-14">/night</span>
            </div>
          </div>
        </div>
        <div class="hotel-card__description-wrapper body-text">
          {{ description }}
        </div>
        <div class="hotel-card__booking-wrapper">
          <div class="hotel-card__booking-wrapper--saving-text">
            You're saving <strong>${{ calcSavedValue() }}</strong>
          </div>
          <button
            class="hotel-card__booking-wrapper--booking-button"
            @click="initBookingRoom"
          >
            select rooms
          </button>
        </div>
      </div>
    </div>
    <div v-if="state.bookingRoom" class="display-flex-column">
      <div class="hotel-card__select-room-container">
        <div class="hotel-card__select-room-container--close-icon">
          <img
            src="../../assets/close-icon.svg"
            alt="Close icon"
            @click="closeBookingArea"
          />
        </div>
        <div class="hotel-card__select-room-container--generic-text">
          <span
            >Any generic information about room selection in
            <strong>{{ title }}</strong></span
          >
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped lang="scss">
@use "../../globals/_globals.scss";

.hotel-card {
  margin: 20px;
  background-color: #ffffff;
  border-radius: 16px;
  box-shadow: rgba(99, 99, 99, 0.2) 0px 2px 8px 0px;
  display: flex;
  flex-direction: column;
  min-width: 430px;
  position: relative;

  &--label {
    display: flex;
    min-height: 32px;
    text-transform: uppercase;
    color: #007ae9;
    font-family: globals.$openSansFont;
    font-weight: 700;
    font-size: 0.625rem;
    text-align: center;
    letter-spacing: 0.2em;
    font-style: normal;
    border: 1px solid #d7d8d9;
    border-radius: 4px;
    align-items: center;
    padding: 0px 10px;
    margin-right: 10px;
  }

  &__image-container {
    max-height: 260px;
    position: relative;
    overflow: hidden;
  }

  &__info-wrapper {
    position: absolute;
    top: 15px;
    right: 20px;
    display: flex;
    align-items: center;
    justify-content: center;
    cursor: pointer;

    &--fav-icon {
      transition: 0.2s;
    }

    &--fav-icon:hover {
      transform: scale(1.2);
    }
  }

  &__info-wrapper img {
    width: 32px;
    height: 32px;
  }

  &__image {
    width: 100%;
    height: 260px;
  }

  &__body-container {
    padding: 20px;
    display: flex;
    flex-direction: column;
    flex-grow: 1;
  }

  &__title-wrapper {
    font-family: globals.$dmSerifDisplay;
    font-size: 1.5rem;
    font-weight: 400px;
    line-height: 25px;
    display: flex;
    flex-direction: row;

    &--title {
      flex-grow: 1;
      color: #081f32;
    }

    &__icons-container {
      display: flex;
    }
  }

  &__infos-wrapper {
    display: flex;
    align-items: end;
    margin-top: 13px;

    img {
      margin-right: 7px;
      width: 16px;
      height: 16px;
    }
  }

  &__left-infos-wrapper {
    display: flex;
    flex-grow: 1;
    flex-direction: column;
    justify-content: center;

    &--secondary {
      display: flex;
      flex-direction: row;
      align-items: center;
      margin-top: 20px;
    }
    &--ranking {
      text-transform: uppercase;
      color: #ffffff;
      border-radius: 4px;
      font-size: 0.625rem;
      font-family: globals.$openSansFont;
      background-color: #007ae9;
      padding: 0px 14px;
      margin-left: 10px;
      font-weight: 700;
      line-height: 22px;
      text-align: center;
      letter-spacing: 0.2em;
      font-style: normal;
    }
  }

  &__right-infos-wrapper {
    display: flex;
    flex-direction: column;
    font-family: globals.$dmSerifDisplay;

    &--discount-price {
      font-style: normal;
      font-weight: 400;
      font-size: 3rem;
      line-height: 25px;
      text-align: right;
      letter-spacing: 0.02em;
      color: #081f32;
    }

    &--original-price {
      margin-top: 2px;
      color: #b1b4b9;
      font-size: 1.375rem;
      line-height: 25px;
      text-align: right;
      letter-spacing: 0.02em;
    }
  }

  &__description-wrapper {
    color: #6e798c;
    display: flex;
    flex-grow: 1;
    max-height: 170px;
  }

  &__booking-wrapper {
    font-family: globals.$openSansFont;
    margin-top: 20px;

    &--saving-text {
      font-size: 0.875rem;
      font-style: normal;
      font-weight: 400;
      line-height: 25px;
      text-align: center;
      letter-spacing: 0.02em;
      color: #2ecc71;
    }

    &--booking-button {
      margin-top: 10px;
      width: 100%;
      text-align: center;
      border-radius: 8px;
      border: none;
      transition: 0.2s;
      height: 80px;
      background-color: #2ecc71;
      font-weight: 700;
      font-size: 17px;
      line-height: 25px;
      text-align: center;
      letter-spacing: 0.2em;
      color: #ffffff;
      text-transform: uppercase;
      cursor: pointer;
    }

    &--booking-button:hover {
      background-color: globals.$buttonHoverBackgroundColor;
    }
  }

  &__select-room-container {
    padding: 20px;
    display: flex;
    flex-direction: column;

    &--close-icon {
      text-align: right;
      cursor: pointer;
    }

    &--generic-text {
      display: flex;
      flex-grow: 1;
      font-family: globals.$openSansFont;
      font-size: 1.85rem;
      text-align: center;
      margin-top: 100px;
    }
  }
}

.icon-default {
  width: 24px;
  height: 24px;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  margin: 0px 5px 0px 5px;
  background-color: #007ae9;
}

.flex-row {
  display: flex;
  flex-direction: row;
  align-items: center;
}

.mt-20 {
  margin-top: 20px;
}

.fs-18 {
  font-size: 18px;
}

.fs-14 {
  font-size: 14px;
}

.line-through {
  text-decoration: line-through;
}

.body-text {
  font-size: 1rem;
  font-family: globals.$openSansFont;
  line-height: 25px;
  margin-top: 25px;
}

.display-flex-column {
  display: flex;
  flex-direction: column;
  flex-grow: 1;
}

@media (max-width: 570px) {
  .hotel-card {
    width: 100%;
    min-width: 100%;

    &__image {
      height: 100%;
    }
    &__title-wrapper {
      flex-direction: column;
      &__icons-container {
        margin-top: 5px;
      }
    }

    &__infos-wrapper {
      flex-direction: column;
      align-items: start;
      margin-top: 8px;
    }

    &__left-infos-wrapper {
      &--secondary {
        margin-top: 10px;
      }
    }

    &__right-infos-wrapper {
      width: 100%;
      margin-top: 5px;

      &--discount-price {
        font-size: 2.5rem;
      }
    }
  }

  .body-text {
    margin-top: 5px;
  }

  .icon-default {
    margin-right: 10px;
    margin-left: 0;
  }
}
</style>
