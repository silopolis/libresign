<template>
	<NcSettingsSection :title="title" :description="description">
		<NcCheckboxRadioSwitch
			type="switch"
			:checked.sync="collectMetadataEnabled"
			@update:checked="saveCollectMetadata()">
			{{ t('libresign', 'Collect signers metadata when signing a document') }}
		</NcCheckboxRadioSwitch>
	</NcSettingsSection>
</template>
<script>
import { translate as t } from '@nextcloud/l10n'
import NcSettingsSection from '@nextcloud/vue/dist/Components/NcSettingsSection'
import NcCheckboxRadioSwitch from '@nextcloud/vue/dist/Components/NcCheckboxRadioSwitch'
import { generateOcsUrl } from '@nextcloud/router'
import axios from '@nextcloud/axios'

export default {
	name: 'CollectMetadata',
	components: {
		NcSettingsSection,
		NcCheckboxRadioSwitch,
	},
	data() {
		return {
			title: t('libresign', 'Collect signers metadata'),
			description: t('libresign', 'Enabling this feature, everytime a document is signed, LibreSign will store the IP address and user agent of the signer.'),
			collectMetadataEnabled: false,
		}
	},
	created() {
		this.getData()
	},
	methods: {
		async getData() {
			const responseCollectMetadata = await axios.get(generateOcsUrl('/apps/provisioning_api/api/v1', 2) + '/config/apps/libresign/collect_metadata', {})
			this.collectMetadataEnabled = responseCollectMetadata.data.ocs.data.data ? true : false
		},
		saveCollectMetadata() {
			OCP.AppConfig.setValue('libresign', 'collect_metadata', this.collectMetadataEnabled ? 1 : 0)
		},
	},
}
</script>
<style scoped>
.collect-metadata-content{
	display: flex;
	flex-direction: column;
}
</style>
