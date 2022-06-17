<template>
	<section>
		<b-nav-item v-b-tooltip.hover title="Save historical data" v-b-modal.historicalDataModal>
			<b-icon-download  class="icon d-none d-lg-inline" /> 
      <span class="d-inline-block d-lg-none">Save historical data</span>
		</b-nav-item>

		<b-modal dialog-class="custom-modal" size="sm" id="historicalDataModal" title="Save Historical Data" centered ok-title="Save" @ok="handleOk" @hidden="resetModal">
			<ValidationObserver ref="formObserver" v-slot="{ handleSubmit }">
				<b-form autocomplete="off" @submit.stop.prevent="handleSubmit">
					<div class="btn-hidden">
						<b-button id="submit_form" type="submit" variant="primary">Submit</b-button>
					</div>
					<div class="grid grid--1">
						<FormElementWithValidation
							v-for="(item, index) in el.MainEl"
							:key="`MainEl${index}`"
							:item="item"
							v-model="vm[item.name]"
						>
						</FormElementWithValidation>
					</div>
				</b-form>
			</ValidationObserver>
		</b-modal>
	</section>
</template>

<script>
export default {
	name: "HistoricalDataForm",
	data: () => ({
		vm: {}
	}),
	computed: {
		el() {
			return getEl(this);
		}
	},
	methods: {
    resetModal(){
      this.vm = {}
    },
		handleOk(bvModalEvt) {
			bvModalEvt.preventDefault();
			this.handleSubmit();
		},
		handleSubmit() {
			this.$refs.formObserver.validate().then(result => {
				if (result == true) {
					this.saveHistoricalData()
				}
			});
		},
    saveHistoricalData(){
        const vObj = {
						dataobj: {
							asondate: this.vm.asondate || []
						}
					};
					this.$store.commit("loading", true);
					this.$credCAPI
						.collection("savehistoricaldata/savedata")
						.create(vObj)
						.then(response => {
							if (response && response.status == "success") {
								this.$_successMessage(response.msg || response.message || "Save historical data processed successfully");
							} else {
								this.$_errorMessage(response.error || `Error while processing`);
							}
							this.$store.commit("loading", false);

							setTimeout(() => {
								this.$bvModal.hide("historicalDataModal");
							}, 1000);
						})
						.catch(error => {
							this.$store.commit("loading", false);
							console.error(JSON.stringify(error));
						});
    }
	}
};

function getEl() {
	return {
		MainEl: [
			{
				type: "date",
				label: "As on date",
				placeholder: "As on date",
				name: "asondate",
				rules: "required"
			}
		]
	};
}
</script>
<style lang="scss">
.custom-modal {
	.modal-content {
		border: 0;
		border-radius: 0;
	}
	.modal-footer {
		border: 0;
	}
	.modal-header {
		background: var(--blue-dark);
		color: var(--white);
		border-radius: 0;
	}
	.close {
		color: var(--white);
		opacity: 0.6;
	}
}
</style>
