<template>
	<div class="fund_content">
		<div class="loading" v-show="isShowLoading">加载中...</div>
		<div class="fund_item" v-for="(item, index) in fundList" :key="index" v-show="!isShowLoading">
			<div class="fund_name fl">
				<div class="fund_name">{{item.name}}</div>
				<div class="fund_desc">AA评级，纳入征信</div>
			</div>
			<div class="fl">
				<div class="fund_price"><span>{{item.funds}}</span>万元</div>
				<div class="fund_desc">起投资金</div>
			</div>
			<div class="fl">
				<div class="fund_limit"><span>{{item.limit}}</span>个月</div>
				<div class="fund_desc">产品期限</div>
			</div>
			<div class="fl">
				<div class="fund_manager">{{item.manager}}</div>
				<div class="fund_desc">资金管理人</div>
			</div>
			<div class="fund_reservation fr">
				<div class="fl">
					<div class="reservation_interest"><span>{{item.rate}}</span>/年</div>
					<p class="reservation_desc">业绩比较基准</p>
					<p class="reservation_desc">筹集状态：{{item.status}}</p>
				</div>
				<div class="reservation_text fr" @click="immediately">预约</div>
			</div>
		</div>
		<div :class="!this.pageShow ? 'fund_consultant bottom50' : 'fund_consultant'" v-show="!isShowLoading">
			资深投资顾问，多年深入研究，只为挑选最合适您的产品<a href="javascript:;" class="immediately" @click="immediately">立即预约咨询</a>
		</div>
		<pagination :total="total" :current-page='pageIndex' :page-size="pageSize" @pageChange="getFixedIncome" v-if="pageShow"></pagination>
	</div>
</template>
<script>
import fundApi from '@/api/sendRequestApi'
import pagination from '@/components/common/pagination'
import Bus from '@/commonJs/Bus'

export default {
	data() {
		return {
			fundList: [],			
			pageIndex: 1,
			pageSize: 2,
			total: 5,
			pageShow: false,
			isShowLoading: false
		}
	},
	mounted(){
		this.getFixedIncome();
	},
	methods: {
		getFixedIncome(currentPage){
			this.isShowLoading = true;
			
			let params = {
				page: currentPage ? currentPage : this.pageIndex,
				limit: this.pageSize
			};
			
			fundApi.GetFundList(params).then(res => {
				this.isShowLoading = false;
				this.pageShow = true;
				this.fundList = res;
			});
		},
		immediately(){
			Bus.$emit("reservationShow", true);
		}
	},
	components: {
		pagination
	}
}
</script>