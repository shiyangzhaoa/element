<template>
  <div class="el-input-ip" :class="[
      {
        'is-disabled': disabled
      }
    ]">
    <span>{{this.currentValue}}</span>
    <div>
      <input type="text" :disabled="disabled" data-index="1" class="el-input-ip__ip" maxlength="3" placeholder="___" :iOne="iOne" @input="handleInput"> .
      <input type="text" :disabled="disabled" data-index="2" class="el-input-ip__ip" maxlength="3" placeholder="___" :iTwo="iTwo" @input="handleInput"> .
      <input type="text" :disabled="disabled" data-index="3" class="el-input-ip__ip" maxlength="3" placeholder="___" :iThree="iThree" @input="handleInput"> .
      <input type="text" :disabled="disabled" data-index="4" class="el-input-ip__ip" maxlength="3" placeholder="___" :iFour="iFour" @input="handleInput">
    </div>
  </div>
</template>
<script>
export default {
  name: 'ElInputIp',

  componentName: 'ElInputIp',

  data() {
    return {
      currentValue: this.value,
      iOne: '',
      iTwo: '',
      iThree: '',
      iFour: ''
    };
  },

  props: {
    value: String,
    disabled: Boolean
  },

  watch: {
    'value'(val, oldValue) {
    }
  },

  methods: {
    handleInput(event) {
      const { index } = event.target.dataset;
      const value = event.target.value;
      this.setCurrentValue(index, value);
      this.$emit('input', this.currentValue);
      this.$emit('change', this.currentValue);
    },
    setCurrentValue(index, value) {
      switch (index) {
        case '1':
          this.iOne = value;
          break;
        case '2':
          this.iTwo = value;
          break;
        case '3':
          this.iThree = value;
          break;
        case '4':
          this.iFour = value;
          break;
        default:
          return;
      }
      this.currentValue = [this.iOne, this.iTwo, this.iThree, this.iFour].filter(v => v).join('.');
    }
  },

  created() {
    console.log(this);
  },

  mounted() {
    const i_inputs = document.querySelectorAll('input.el-input-ip__ip');
    i_inputs.forEach(el => {
      el.addEventListener('keypress', evt => {
        const currentValue = +`${el.value.slice(0, el.selectionStart)}${evt.key}${el.value.slice(el.selectionEnd)}`;
        if (
          !/\d/.test(evt.key) ||
          (el.value === '0' && el.selectionStart === 1) ||
          currentValue > 255
        ) {
          evt.preventDefault();
          el.nextElementSibling && el.nextElementSibling.focus();
        }
      });
      el.addEventListener('input', evt => {
        el.dataset.length = el.value.length;
      });
    });
  }
};
</script>
