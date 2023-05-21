<template>
  <div v-key-bind-listen>
      <div class="msgBox" v-show="isShowMessageBox">
        <div class="msgBox_header">
          <div class="msgBox_title">
            <h3>{{ title }}</h3>
          </div>
        </div>

        <div class="msgBox_content">
          <p>{{ content }}</p>
        </div>

        <div class="msgBox_btns">
          <button type="button" class="btn btn-lime btn-lg" id="confirmBtn" @click="confirm"  bind_key="ENTER">确定</button>
          <button type="button" class="btn btn-dark btn-lg" id="cancelBtn" @click="cancel"  bind_key="ESC">取消</button>

        </div>

      </div>
  </div>
</template>

<script>
  export default {
    name: 'messageBox',
    data(){
      return {
        title: '',
        content: '',
        isShowMessageBox: false,
        resolve: '',
        reject: '',
        promise: '' // 保存promise对象
      }
    },
    methods: {
      close(state){
        this.model.show = false;
        if(this.model.callback){
          this.model.callback(state);
        }
      },
    // 确定,将promise断定为resolve状态
    confirm: function () {
      this.isShowMessageBox = false;
      this.resolve('confirm');
      this.remove();
    },
    // 取消,将promise断定为reject状态
    cancel: function () {
      this.isShowMessageBox = false;
      this.reject('cancel');
      this.remove();
    },
    // 弹出messageBox,并创建promise对象
    showMsgBox: function () {
      this.isShowMessageBox = true;
      this.promise = new Promise((resolve, reject) => {
        this.resolve = resolve;
        this.reject = reject;
      });
      // 返回promise对象
      return this.promise;
    },
    remove: function () {
      setTimeout(() => {
        this.destroy();
      }, 300);
    },
    destroy: function () {
      this.$destroy();
      if (document.body.contains(this.$el)) {
        document.body.removeChild(this.$el);
      }
    }
</script>

<style scoped>
  .msgBox {
    position: fixed;
    z-index: 4;
    left: 50%;
    top: 35%;
    transform: translateX(-50%);
    width: 420px;
    background-color: black;
    opacity: 0.55;
  }

  .msgBox_header {
    padding: 20px 20px 0;
  }

  .msgBox_title {
    padding-left: 0;
    margin-bottom: 0;
    font-size: 26px;
    font-weight: 800;
    height: 18px;
    color: #fff;
  }

  .msgBox_content {
    padding: 30px 20px;
    color: #fff;
    font-size: 18px;
    font-weight: 200;
  }

  .msgBox_btns {
    padding: 10px 20px 15px;
    text-align: right;
    overflow: hidden;
  }

  @keyframes show-messageBox {
    from {
      opacity: 0;
    }
    to {
      opacity: 1;

    }
  }

  @keyframes bounce-in {
    from {
      opacity: 0;
    }
    to {
      opacity: 1;

    }
  }


</style>
