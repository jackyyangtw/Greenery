<template>
  <div class="p-5">
    <h2 class="text-center pt-3">您的訂單</h2>
    <div class="pt-5 pb-5 m-auto col-md-6 col-sm-10 col-10 row justify-content-center order_list">
      <form class="col-md-10 col-sm-10 col-10" @submit.prevent="payOrder">
        <table class="table">
          <thead>
            <th>品名</th>
            <th>數量</th>
            <th>單價</th>
          </thead>
          <tbody>
            <tr v-for="item in order.products" :key="item.id">
              <td class="align-middle">{{ item.product.title }}</td>
              <td class="align-middle">{{ item.qty }}/{{ item.product.unit }}</td>
              <td class="align-middle text-right">{{ item.final_total }}</td>
            </tr>
          </tbody>
          <tfoot>
            <tr>
              <td colspan="2" class="text-right">總計</td>
              <td class="text-right">{{ order.total }}</td>
            </tr>
          </tfoot>
        </table>

        <table class="table">
          <tbody>
            <tr>
              <th width="100">Email</th>
              <td>{{ order.user.email }}</td>
            </tr>
            <tr>
              <th>姓名</th>
              <td>{{ order.user.name }}</td>
            </tr>
            <tr>
              <th>收件人電話</th>
              <td>{{ order.user.tel }}</td>
            </tr>
            <tr>
              <th>收件人地址</th>
              <td>{{ order.user.address }}</td>
            </tr>
            <tr>
              <th>付款狀態</th>
              <td>
                <span v-if="!order.is_paid">尚未付款</span>
                <span v-else class="text-success">付款完成</span>
              </td>
            </tr>
          </tbody>
        </table>
        <div class="text-right" v-if="order.is_paid === false">
          <button class="btn btn-warning">確認付款去</button>
        </div>
      </form>
    </div>
    <div class="modal fade" id="successAlert" tabindex="-1" role="dialog" aria-labelledby="exampleModalLabel" aria-hidden="true">
      <div class="modal-dialog" role="document">
        <div class="modal-content">
          <div class="modal-header">
            <i class="fas fa-check text-success icon"></i>
            <button type="button" class="close" data-dismiss="modal" aria-label="Close">
              <span aria-hidden="true">&times;</span>
            </button>
          </div>
          <div class="modal-body mb-5">
            感謝您的購買，已成功付款!
          </div>
          <div class="d-flex justify-content-center align-items-center">
            <!-- 不可使用router-link會導致router和modal同時作用，會出錯 -->
            <button class="btn btn-primary text-dark mr-2 " @click="hideModal('shop')">
              繼續看看
            </button>
            <button class="btn btn-primary text-dark" @click="hideModal('home')">
              回首頁
            </button>
          </div>
          <button class="btn"></button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import $ from 'jquery'
export default {
  data(){
    return{
      order: {
        //注意資料結構，否則出錯
        user:{}
      },
      orderId: '',
    }
  },
  methods:{
    getOrder(){
      const vm = this
      const api = `${process.env.VUE_APP_APIPATH}/api/${process.env.VUE_APP_CUSTOMPATH}/order/${vm.orderId}`
      vm.isLoading = true
      this.$http.get(api).then((response)=>{
        console.log(response)
        vm.order = response.data.order
        vm.isLoading = false
      })         
    },
    payOrder(){
      const vm = this
      const api = `${process.env.VUE_APP_APIPATH}/api/${process.env.VUE_APP_CUSTOMPATH}/pay/${vm.orderId}`
      vm.isLoading = true
      this.$http.post(api).then((response)=>{
        console.log(response)
        vm.isLoading = false
        vm.$store.dispatch('cartsModules/getCart')
        if(response.data.success){
          $('#successAlert').modal('show')
        }
      })        
    },
    hideModal(url){
      $('#successAlert').modal('hide')
      if(url === 'home'){
        this.$router.push('/home')

      } else {
        this.$router.push('/shop')
      }
    }
  },
  created(){
    //this.$router.params.orderId 的 orderId必須和router設定的path名稱一樣
    //注意$route不能打成$router
    this.orderId = this.$route.params.orderId
    console.log(this.orderId)
    this.getOrder()
  },
}
</script>

<style lang="sass" scoped>
@import '@/assets/sass/FrontCheckout.sass'
.icon
  font-size: 50px
</style>