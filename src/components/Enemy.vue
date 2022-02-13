<template>
  <div>
    <div v-if="!enemyIsHit" class="hitbox">
      <img class="hitbox--image"
           src="../assets/images/png/manStanding.png"
           alt=""
      >
      <div class="hitbox--box"
           @click="shoot()"/>
    </div>
    <div v-else class="hitbox">
      <img class="hitbox--image"
           src="../assets/images/png/manShooting.png"
           alt=""
      >
    </div>
  </div>
</template>

<script>
export default {
  name: 'Enemy',
  data() {
    return {
      enemyIsHit: false,
    }
  },
  methods: {
    shoot() {
      this.enemyIsHit = true;
    },
  }
}
</script>

<style lang="scss" scoped>
$block-height: 150px;
$block-width: calc(#{$block-height} / 350 * 104);

.hitbox {
  position: relative;
  width: $block-width;
  height: $block-height;
  display: flex;
  justify-content: center;
  align-items: center;

  &--image {
    height: $block-height;
    object-fit: cover;
  }

  &--box {
    position: absolute;
    width: 90%;
    height: 100%;
    border-radius: 50%;
    margin-right: 2%;
  }
}
</style>
