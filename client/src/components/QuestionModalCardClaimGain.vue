<template>
	<div class="modal-card" style="min-width:400px;">
		<header class="modal-card-head">
			<p class="modal-card-title">{{$t('claimModalTitle')}}</p>
		</header>
		<section class="modal-card-body">
			<div>
				<h4 class="title is-4" > {{question.question}} </h4>
				<hr>
			</div>
				<div v-if="!link">
					<div v-if="question.unconfirmedEvents" >
						<unconfirmed-events :unconfirmedEvents="question.unconfirmedEvents" />
					</div>
					<div class="p-2">{{$t('claimSelectAddress')}}</div>
					<div class="pb-1">
						<div v-for="(address,index) in question.claimAddresses" :key="index" class="level">
							<div class="level-item"/>
							<b-button type="is-primary level-item" :outlined="selectedAddress!=address" @click="selectedAddress=address">{{address}}</b-button>
							<div class="level-item"/>
						</div>
					</div>
					<question-history :question="question" />
				</div>
				<div v-else>
					<p class="mt-2">{{$t('claimGainLinkHeader')}}</p>
						<icon-link :link="link" />
					<p class="mt-1">{{$t('claimGainLinkFooter')}}</p>
				</div>
		</section>
		<footer class="modal-card-foot">
			<button class="button" type="button" @click="$emit('close')">{{$t('commonButtonClose')}}</button>
			<button v-if="selectedAddress&&!link" class="button is-primary" type="button"  @click="handleOk">{{$t('commonButtonCreateLink')}}</button>
		</footer>
	</div>
	
</template>

<script>
const conf = require("../conf.js")
import ByteAmount from './commons/ByteAmount.vue'
import QuestionHistory from './commons/QuestionHistory.vue';
import UnconfirmedEvents from './commons/UnconfirmedEvents.vue';
import IconLink from './commons/IconLink.vue'

export default {
	components: {
		QuestionHistory,
		UnconfirmedEvents,
		IconLink
	},
	props: {
		question: {
			type: Object,
			required: true
		}
	},
	data(){
		return {
			link: null,
			selectedAddress: null
		}
	},
	watch:{
		selectedAddress: function(){

		}
	},
	created(){

	},
	methods:{
		handleOk:function(outcome){
			const data = {
				question_id: this.question.question_id,
				withdraw: true,
				address: this.selectedAddress
			};

			const json_string = JSON.stringify(data);
			const base64data = encodeURIComponent(btoa(json_string));
			this.link = conf.protocol+":"+conf.aa_address+"?amount=10000&base64data="+base64data;
		}
	}
}
</script>

<style lang='scss' scoped>

</style>

