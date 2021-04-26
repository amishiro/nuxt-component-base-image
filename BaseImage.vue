<template>
  <figure class="base-image">
    <!--
      遅延読み込み画像
      素早く上方向スクロールした際にちらつくのでrootMarginの上方は大きめの値を指定
     -->
    <VLazyImage
      v-if="lazy"
      class="base-image__image"
      :src="src"
      src-placeholder="data:image/gif;base64,R0lGODlhAQABAGAAACH5BAEKAP8ALAAAAAABAAEAAAgEAP8FBAA7"
      :alt="alt"
      :width="width"
      :height="height"
      :intersection-options="{
        rootMargin: '300px 0px 0px 0px',
      }"
    />

    <!--
      遅延読み込みさせない画像
      ファーストビューなど表示速度優先の場合に利用
      :lazy="false"を渡すことで利用可
    -->
    <img
      v-else
      class="base-image__image"
      :src="src"
      :alt="alt"
      :width="width"
      :height="height"
    />

    <!-- キャプション -->
    <figcaption
      v-if="caption"
      class="base-image__caption"
      :class="[addClassType]"
    >
      <!-- eslint-disable-next-line vue/no-v-html -->
      <span v-html="caption" />
    </figcaption>
  </figure>
</template>

<script>
// docs: https://www.npmjs.com/package/v-lazy-image
import VLazyImage from 'v-lazy-image'

export default {
  components: {
    VLazyImage,
  },

  props: {
    // 画像URL
    src: {
      type: String,
      default: undefined,
    },

    // alt
    alt: {
      type: String,
      default: undefined,
    },

    // キャプション
    caption: {
      type: String,
      default: undefined,
    },

    // 横幅サイズ
    // レイアウトシフトを回避するため必須
    width: {
      type: String,
      default: undefined,
      require: true,
    },

    // 縦幅サイズ
    // レイアウトシフトを回避するため必須
    height: {
      type: String,
      default: undefined,
      require: true,
    },

    // 遅延読み込みをする・しないをコントロール(ディフォルト: true)
    // ファーストビュー内は表示速度優先でfalseを指定する
    lazy: {
      type: Boolean,
      default: true,
    },
  },
}
</script>

<style lang="scss" scoped>
.base-image {

  // .base-image__image

  &__image {
    // 画像ロード後、.3秒フェードイン
    &.v-lazy-image {
      opacity: 0;

      &-loaded {
        opacity: 1;
        transition: opacity 0.3s;
      }
    }
  }

  // .base-image__caption

  &__caption {
    font-size: 1.1rem;
    font-weight: normal;
    font-feature-settings: 'palt';
    text-align: right;
  }
}
</style>
