<script setup lang="ts">
import { computed } from 'vue';
import type { CSSProperties } from 'vue';
import Icon from '../ai-icon/ai-icon.vue';
import type { IconName } from '../ai-icon/types';
import { useNow } from '../../composables/useNow';
import { usePhoneScale } from '../../composables/usePhoneScale';

interface AppDef {
  id: string;
  iconName: IconName;
  color: string;
  offset?: boolean;
  hasNewMessage?: boolean;
  iconStyle?: CSSProperties;
}

const apps: AppDef[] = [
  { id: 'camera', iconName: 'icon-camera', color: '#B77DEE', hasNewMessage: true },
  { id: 'app', iconName: 'icon-miles', color: '#889DF0', offset: true },
  { id: 'critterpedia', iconName: 'icon-critterpedia', color: '#F7CD67', iconStyle: { width: '210rpx' } },
  { id: 'diy', iconName: 'icon-diy', color: '#E59266' },
  { id: 'shopping', iconName: 'icon-design', color: '#F8A6B2' },
  { id: 'variant', iconName: 'icon-map', color: '#82D5BB', hasNewMessage: true, iconStyle: { width: '180rpx' } },
  { id: 'design', iconName: 'icon-variant', color: '#8AC68A', iconStyle: { width: '160rpx' } },
  { id: 'map', iconName: 'icon-helicopter', color: '#FC736D' },
  { id: 'chat', iconName: 'icon-chat', color: '#D1DA49' },
];

const { now } = useNow();

const display = computed(() => {
  const h = now.value.getHours();
  const m = now.value.getMinutes();
  return {
    hours: (h % 12 || 12).toString(),
    minutes: m.toString().padStart(2, '0'),
    ampm: h >= 12 ? 'PM' : 'AM',
  };
});

const iconStyleFor = (app: AppDef): CSSProperties => ({
  backgroundSize: '70% auto',
  ...(app.iconStyle || {}),
});

const { scalerStyle, phoneStyle } = usePhoneScale();
</script>

<template>
  <view class="animal-phone-wrap">
    <view class="animal-phone-stage">
      <view class="animal-phone-scaler" :style="scalerStyle">
        <view class="animal-phone" :style="phoneStyle">
          <view class="animal-phone__inner">
            <view class="animal-phone__home">
              <view class="animal-phone__statusbar">
                <view class="animal-phone__topbar">
                  <text class="animal-phone__icon animal-phone__icon--wifi" />
                  <view class="animal-phone__time">
                    {{ display.hours }}<text class="animal-phone__blink">:</text>{{ display.minutes }}{{ display.ampm }}
                  </view>
                  <text class="animal-phone__icon animal-phone__icon--loc" />
                </view>
                <view class="animal-phone__welcome">Welcome!</view>
              </view>
              <view class="animal-phone__grid">
                <view
                  v-for="app in apps"
                  :key="app.id"
                  class="animal-phone__app"
                  :class="{ 'animal-phone__app--clip': app.offset }"
                  :style="{ backgroundColor: app.color }"
                >
                  <text v-if="app.hasNewMessage" class="animal-phone__badge" />
                  <Icon
                    :name="app.iconName"
                    size="100%"
                    class="animal-phone__app-icon"
                    :class="{ 'animal-phone__app-icon--offset': app.offset }"
                    :style="iconStyleFor(app)"
                  />
                </view>
              </view>
              <view class="animal-phone__pageindicator">
                <text class="animal-phone__icon animal-phone__icon--page" />
              </view>
            </view>
          </view>
        </view>
      </view>
    </view>
  </view>
</template>

<style lang="less" scoped>
.animal-phone-wrap {
  display: flex;
  justify-content: center;
  align-items: center;
  max-width: 100%;
  overflow: hidden;
  padding: 20rpx;
  box-sizing: border-box;
}

/* Stage fills the wrap's content box; the scaler (sized in JS) centers in it. */
.animal-phone-stage {
  width: 100%;
  display: flex;
  justify-content: center;
}

/* Reserves the scaled device box so the absolutely-positioned, transformed
   phone never causes layout overflow on narrow screens. */
.animal-phone-scaler {
  position: relative;
}

.animal-phone {
  position: absolute;
  border-radius: 272rpx;
  overflow: hidden;
  background: #f8f4e8;

  &__inner {
    overflow: hidden;
  }

  &__home {
    height: 100%;
    display: flex;
    flex-direction: column;
    align-items: center;
    padding-top: 80rpx;
    background: #f8f4e8;
    background-size: 100% 200%;
    animation: animal-phone-grasswave 8s ease-in-out infinite;
  }

  &__statusbar {
    text-align: center;
    width: 100%;
    padding: 0 140rpx 62rpx;
    font-family: var(--font-animal, inherit);
  }

  &__topbar {
    display: flex;
    justify-content: space-between;
    align-items: center;
    font-size: 64rpx;
    color: #dddbcc;
    font-weight: 800;
    letter-spacing: 4rpx;
  }

  &__time {
    display: flex;
    align-items: baseline;
  }

  &__blink {
    font-size: 64rpx;
    font-weight: 800;
    color: #dddbcc;
    animation: animal-phone-blink 1s steps(1) infinite;
  }

  &__welcome {
    display: block;
    font-size: 96rpx;
    font-weight: 800;
    height: 112rpx;
    margin-top: 40rpx;
    color: #725c4e;
    letter-spacing: 4rpx;
  }

  &__grid {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    gap: 64rpx;
    padding: 16rpx;
    flex: 1;
    align-items: center;
    justify-content: center;
    align-content: center;
  }

  &__app {
    position: relative;
    display: flex;
    justify-content: center;
    align-items: center;
    width: 246rpx;
    height: 246rpx;
    border-radius: 90rpx;

    &--clip {
      overflow: hidden;
    }

    &:hover :deep(.animal-phone__app-icon) {
      animation: animal-phone-bounce 0.3s ease-in-out forwards;
    }
    &:hover :deep(.animal-phone__app-icon--offset) {
      animation: animal-phone-bounce-offset 0.3s ease-in-out forwards;
    }
  }

  &__badge {
    position: absolute;
    top: 0;
    left: 0;
    width: 56rpx;
    height: 56rpx;
    border-radius: 50%;
    background: #ff544a;
    border: 5px solid #f8f4e8;
    z-index: 1;
  }

  :deep(.animal-phone__app-icon) {
    &.animal-phone__app-icon--offset {
      transform: translateY(30rpx);
    }
  }

  &__pageindicator {
    display: flex;
    justify-content: center;
    margin-top: 148rpx;
    align-items: center;
  }

  &__icon {
    display: inline-block;

    &--wifi {
      width: 158rpx;
      height: 58rpx;
      background: url('../../assets/img/icons/wifi.svg') center / contain no-repeat;
    }
    &--loc {
      width: 72rpx;
      height: 72rpx;
      background: url('../../assets/img/icons/location.svg') center / contain no-repeat;
    }
    &--page {
      width: 130rpx;
      height: 64rpx;
      background: url('../../assets/img/icons/page.svg') center / contain no-repeat;
    }
  }
}

@keyframes animal-phone-grasswave {
  0%,
  100% {
    background-position: 0% 0%;
  }
  50% {
    background-position: 0% 100%;
  }
}

@keyframes animal-phone-blink {
  0%,
  50% {
    opacity: 1;
  }
  51%,
  100% {
    opacity: 0;
  }
}

@keyframes animal-phone-bounce {
  0% {
    transform: scale(1) rotate(0deg);
  }
  50% {
    transform: scale(1.2) rotate(-5deg);
  }
  100% {
    transform: scale(1.1) rotate(-4deg);
  }
}

@keyframes animal-phone-bounce-offset {
  0% {
    transform: scale(1) rotate(0deg) translateY(30rpx);
  }
  50% {
    transform: scale(1.2) rotate(-5deg) translateY(30rpx);
  }
  100% {
    transform: scale(1.1) rotate(-4deg) translateY(30rpx);
  }
}
</style>
