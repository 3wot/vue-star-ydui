<template>
	<div class="send-deal">
		
		 <yd-layout>
	       <!-- 头 -->
			<yd-navbar slot="navbar" title="提交收放款凭证" fontsize=".4rem" bgcolor="#5871f5" color="#ffffff">
				<span slot="left" @click="gotoIndex">
					<yd-navbar-back-icon color="#ffffff"></yd-navbar-back-icon>
				</span>
			</yd-navbar>

			<div class="slot-bottom" slot="bottom">
				<yd-flexbox>

        	 		<yd-button class="bottom-btn" size="large" @click.native="sub">提交</yd-button>	
        	 		

		        </yd-flexbox>
			</div>

			<!-- 内容 -->
			<ImgUpload required="1" title="现场照片" :arr="LoanReceivedImageUrls" :arrc="C_LoanReceivedImageUrls"></ImgUpload>
			
	    </yd-layout>



	</div>
</template>

<script>
import URLS from '../router/link'
import ImgUpload from './ImgUpload'

export default {
	components:{
		ImgUpload
	},
	name: 'SendDeal',
	data () {
		return {
			"LoanReceivedImageUrls" : [],
			"C_LoanReceivedImageUrls" : []
		}
	},
	mounted () {
		
	},
	methods:{
		gotoIndex() {
			const { id, hid } = this.$route.params
			this.$dialog.confirm({
                title: '提示',
                mes: '当前页面如有修改，将会丢失，你确定退出吗？',
                opts: () => {
                    // 跳到首页
					this.$router.push({ name : 'opList',params: { id, hid }})
                }
            });
		},

		// 确认
		sub () {
			const { id, hid, oprid } = this.$route.params
			const {
				LoanReceivedImageUrls,
				C_LoanReceivedImageUrls,
			} = this
			const param = {
				OrderId: id,
				OperationRecordId: oprid,
				LoanReceivedImageUrls,
				C_LoanReceivedImageUrls,
			}
			if (LoanReceivedImageUrls.length && C_LoanReceivedImageUrls.length) {

			} else {
				this.$dialog.toast({
						mes: "请上传照片！",
						icon: 'none',
						timeout: 3000,
					})
				return
			}
			let UPLOAD_TEMP = false
			Object.keys(UPLOAD_NUM).forEach(k => {
				if (UPLOAD_NUM[k]) {
					UPLOAD_TEMP = true
				}
			})
			if (UPLOAD_TEMP) {
				this.$dialog.toast({
					mes: '还有图片正在上传！',
					icon: 'none',
					timeout: 3000,
				})
				return
			}
			this.pp('CompleteUploadLoanReceivedCertificate', param, res => {
				if (res.ret) {
					// 跳到操作页面
					this.$router.push({ name : 'opList', params: { id, hid }})
				} else {
					this.$dialog.toast({
						mes: res.msg,
						icon: 'none',
						timeout: 3000,
					})
				}
			})
		},

	},


}
</script>

<style scoped>
.send-deal {
	position: absolute;
	top: 0px;
	left: 0px;
	right: 0px;
	bottom: 0px;
	overflow: hidden;
	vertical-align: middle;
}


</style>
