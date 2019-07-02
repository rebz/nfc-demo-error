<template>
    <Page>
        <ActionBar title="Welcome to NativeScript-Vue!"/>
		<StackLayout row="0">
			<Label :text="message" />
			<Label text=" " />
			<Label text=" " />
			<Label text=" " />
			<Label text="Start Listening" @tap="nfcStartListening" />
			<Label text=" " />
			<Label :text="moment" />
			<Label text=" " />
			<Label text=" " />
		</StackLayout>
    </Page>
</template>

<script >
	const Nfc = require("nativescript-nfc").Nfc;

	export default {
		created() {
			
			this.nfc = new Nfc();

			// this.nfc.available().then(function(avail) {
			// 	this.message = avail ? "Yes" : "No"
			// });
		},
		data() {
			return {
				nfc: null,
				message: 'loading',
				moment: null
			}
		},
		methods: {
			nfcStartListening() {
				this.message = 'listening'
				this.nfc.setOnNdefDiscoveredListener(async data => {

					if (data.message) {
						let tags = [];
						data.message.forEach(record => {
							const msg = JSON.parse(record.payloadAsString)
							tags.push(msg)
						});
						const moment = tags[0]
						this.moment = moment
						this.message = 'done'
					}
				}, {
					stopAfterFirstRead: true,
					scanHint: 'scan me'
				})
					.catch(err => alert(err));
			},

		}
	}
</script>

<style scoped>
    ActionBar {
        background-color: #53ba82;
        color: #ffffff;
    }

    .message {
        vertical-align: center;
        text-align: center;
        font-size: 20;
        color: #333333;
    }
</style>
