<template>
  <button class="forest-button" :class="classes" :disabled="disabled" @click="$emit('click', $event)">
    <span v-if="loading" class="forest-loadingIndicator"></span>
    <slot/>
  </button>
</template>
<script lang="ts" setup="props">
import {computed} from 'vue';

const props = withDefaults(defineProps<{
  theme?: 'button' | 'text' | 'link';
  size?: 'normal' | 'big' | 'small';
  level?: 'normal' | 'main' | 'danger';
  disabled?: boolean;
  loading?: boolean;
}>(), {
  theme: 'button'
});
const {theme, size, level} = props;

defineEmits<{
  (e: 'click', event: MouseEvent): void
}>();
const classes = computed(() => {
  return {
    [`forest-theme-${theme}`]: theme,
    [`forest-size-${size}`]: size,
    [`forest-level-${level}`]: level,
  };
});
</script>
<style lang="scss">
$h: 32px;
$border-color: #d9d9d9;
$color: #333;
$green: #42b983;
$radius: 4px;
$red: red;
$grey: grey;

.forest-button {
  box-sizing: border-box;
  height: $h;
  padding: 0 12px;
  cursor: pointer;
  display: inline-flex;
  justify-content: center;
  align-items: center;
  white-space: nowrap;
  background: white;
  color: $color;
  border: 1px solid $border-color;
  border-radius: $radius;
  box-shadow: 0 1px 0 fade-out(black, 0.95);
  transition: background 250ms;

  & + & {
    margin-left: 8px;
  }

  &:hover,
  &:focus {
    color: $green;
    border-color: $green;
  }

  &:focus {
    outline: none;
  }

  &::-moz-focus-inner {
    border: 0;
  }

  &.forest-theme-link {
    border-color: transparent;
    box-shadow: none;
    color: $green;

    &:hover,
    &:focus {
      color: lighten($green, 10%);
    }
  }

  &.forest-theme-text {
    border-color: transparent;
    box-shadow: none;
    color: inherit;

    &:hover,
    &:focus {
      background: darken(white, 5%);
    }
  }

  &.forest-size-big {
    font-size: 24px;
    height: 48px;
    padding: 0 16px;
  }

  &.forest-size-small {
    font-size: 12px;
    height: 20px;
    padding: 0 4px;
  }

  &.forest-theme-button {
    &.forest-level-main {
      background: $green;
      color: white;
      border-color: $green;

      &:hover,
      &:focus {
        background: darken($green, 10%);
        border-color: darken($green, 10%);
      }
    }

    &.forest-level-danger {
      background: $red;
      border-color: $red;
      color: white;

      &:hover,
      &:focus {
        background: darken($red, 10%);
        border-color: darken($red, 10%);
      }
    }
  }

  &.forest-theme-link {
    &.forest-level-danger {
      color: $red;

      &:hover,
      &:focus {
        color: darken($red, 10%);
      }
    }
  }

  &.forest-theme-text {
    &.forest-level-main {
      color: $green;

      &:hover,
      &:focus {
        color: darken($green, 10%);
      }
    }

    &.forest-level-danger {
      color: $red;

      &:hover,
      &:focus {
        color: darken($red, 10%);
      }
    }
  }

  &.forest-theme-button {
    &[disabled] {
      cursor: not-allowed;
      color: $grey;

      &:hover {
        border-color: $grey;
      }
    }
  }

  &.forest-theme-link,
  &.forest-theme-text {
    &[disabled] {
      cursor: not-allowed;
      color: $grey;
    }
  }

  > .forest-loadingIndicator {
    width: 14px;
    height: 14px;
    display: inline-block;
    margin-right: 4px;
    border-radius: 8px;
    border-color: $green $green $green transparent;
    border-style: solid;
    border-width: 2px;
    animation: forest-spin 1s infinite linear;
  }
}

@keyframes forest-spin {
  0% {
    transform: rotate(0deg)
  }

  100% {
    transform: rotate(360deg)
  }
}
</style>
