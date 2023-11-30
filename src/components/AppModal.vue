<template>
  <div
    class="custom-modal"
    v-if="isModalOpen"
    @keydown.enter.prevent="saveHandler"
  >
    <div class="custom-modal-header">
      <h3>Установка пользовательского отображения</h3>
      <button class="close" @click.prevent="closeModal">&times;</button>
    </div>
    <div class="custom-modal-helper">
      <p>
        Чтобы передать значение нужно прописать {value} пример кода: {{
        "<i>{value}</i>" }}
      </p>
    </div>
    <div class="custom-modal-body">
      <textarea v-model="userCode" placeholder="Введите код сюда "></textarea>
      <small class="error" v-if="errorText">{{ errorText }}</small>
    </div>

    <div class="custom-modal-footer">
      <button class="btn" @click.prevent="$emit('close')">Отменить</button>
      <button class="btn" @click.prevent="saveHandler">Сохранить</button>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    isModalOpen: {
      type: Boolean,
      required: true,
    },
    value: {
      type: String,
      default() {
        return '';
      },
    },
  },
  data() {
    return {
      userCode: '',
      errorText: '',
    };
  },

  methods: {
    closeModal() {
      this.$emit('close');
      this.userCode = '';
    },
    saveHandler() {
      this.errorText = false;
      if (!this.isValidCode(this.userCode)) {
        return;
      }
      this.$emit('save', this.userCode);
      this.userCode = '';
    },
    isValidCode(code) {
      if (!code.trim()) {
        this.errorText = 'Введите код!';
        return false;
      }
      if (
        code.includes('eval') ||
        code.includes('console') ||
        code.includes('alert')
      ) {
        this.errorText = 'В коде существуют запрещенные ключевые слова!!';
        return false;
      }
      if (!code.includes('{value}')) {
        this.errorText = 'Вы не передали значение столбца!';
        return false;
      }
      return true;
    },
  },
  watch: {
    value(val) {
      if (!val) {
        return;
      }
      this.userCode = val;
    },
  },
};
</script>
<style scoped>
.custom-modal {
  box-shadow: 0px 1px 12px rgba(0, 0, 0, 0.4);
  left: calc(50vw - 300px);
  position: absolute;
  z-index: 999;
  width: 600px;
  top: 20vh;
  border-radius: 5px;
  overflow: hidden;
}
.btn {
  border: none;
  outline: none !important;
  padding: 8px 12px;
  border-radius: 5px;
  background-color: #5c4084;
  color: #fff;
  cursor: pointer;
}
.custom-modal-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 8px 12px;
  background-color: #eee;
}
.custom-modal-helper {
  padding: 8px 12px;
  background-color: #eee;
  font-size: 0.8em;
}
.close {
  cursor: pointer;
  border: none;
  background: none;
  font-size: 1.5em;
  line-height: 1;
  color: #5c4084;
}

.custom-modal .custom-modal-body {
  background-color: #fff;
  padding: 8px 12px;
  text-align: left;
  padding: 12px;
}
.custom-modal-body textarea {
  width: 100%;
  height: 100%;
  border: none;
  outline: none;
  resize: none;
}
.custom-modal .custom-modal-footer {
  display: flex;
  justify-content: flex-end;
  align-items: center;
  gap: 12px;
  background-color: #eee;
  padding: 4px 12px;
  text-align: left;
  padding: 12px;
}
.error {
  color: red;
}
</style>