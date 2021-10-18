<template>
    <div class="popup" v-bind:class='getClassPopup'>
      <div class="rule">
        <h3>Luật Chơi</h3>
      <p>Chơi rồi biết!!! <br/> Có thể thay đổi điểm mục tiêu</p>
      <button
      v-on:click='confirm'
      class="confirm">Đã hiểu</button>
      </div>
    </div>
</template>

<script>
export default {
  name: 'popup',
  props: {
    isOpenPopup: Boolean
  },
  data () {
    return {}
  },
  methods: {
    confirm () {
      this.$emit('handleConfirm')
    }
  },
  computed: {
    getClassPopup () {
      return {
        'open-popup': this.isOpenPopup
      }
    }
  }
}
</script>

<style>
  .popup {
    position: fixed;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    background-color: rgba(0, 0, 0, 0);
    opacity: 0;
    visibility: hidden;
    transition: all .3s ease;
  }
  .popup.open-popup {
    opacity: 1;
    visibility: visible;
  }
  .rule {
    width: 350px;
    padding: 20px;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%) scale(1.2);
    background-color: #fff;
    position: absolute;
    font-family: Arial, Helvetica, sans-serif;
    transition: all .3s ease;
  }
  .open-popup .rule {
    transform: translate(-50%, -50%) scale(1);
  }
  .rule h3 {
    margin-bottom: 10px;
  }
  .rule .confirm {
    cursor: pointer;
    margin-top: 20px;
    padding: 8px 15px;
    border: 2px solid #333;
    background-color: #fff;
    transition: all .3s ease;
  }
  .rule .confirm:hover {
    color: #fff;
    background-color: #333;
  }
  /*CSS3 vs Vue=> Zoom in zoom out (scale) */
</style>
