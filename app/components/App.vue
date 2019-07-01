<template>
    <Page>
        <ActionBar title="Welcome to NativeScript-Vue!"/>
		<StackLayout row="0">
			<Label :text="message" />
		</StackLayout>
    </Page>
</template>

<script >
	const Nfc = require("nativescript-nfc").Nfc;

	export default {
		created() {
			this.nfc = new Nfc();
			this.nfc.available().then(function(avail) {
				this.message = avail ? "Yes" : "No"
			});
		},
		data() {
			return {
				nfc: null,
				message: 'loading'
			}
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
