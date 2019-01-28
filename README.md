# Vue Cloudinary Upload
Simple wrapper to upload files using cloudinary api
## Usage
### Locally
```js
import VCloudinaryUpload from 'v-cloudinary-upload'

export default {
  components: { VCloudinaryUpload }
}
```
### Globally
```js
import Vue from 'vue'
import * as VCloudinaryUpload from 'v-cloudinary-upload'

Vue.use(VCloudinaryUpload)
```
### Examples
#### Unsigned upload
```html
<v-cloudinary-upload autoUpload accountName="skyatura" preset="foo">
	<button>Click here to select a file</button>
</v-cloudinary-upload>
```
#### Unsigned upload with preview
```html
<v-cloudinary-upload autoUpload accountName="skyatura" preset="foo">
	<template slot-scope="props">
		<img :src="props.url" v-if="props.url">
		<button v-else>Click here to select a file</button>
	</template>
</v-cloudinary-upload>
```
#### Signed upload
```html
<template>
	<v-cloudinary-upload ref="img" accountName="skyatura" @picked="onPickFile">
		<button>Click here to select a file</button>
	</v-cloudinary-upload>
</template>

<script>
export default {
	methods: {
		async onPickFile() {
			const params = await this.$axios.$get('/upload/userPicture')
			this.$refs.img.upload(params)
		}
	}
}
</script>
```
## Properties
| Property | Required | Type | Description |
|--|--|--|--|
| accountName | **true** | String | Your Cloudinary's account name
| autoUpload | *false* | Boolean | If the component should automatically perform an upload after selecting a file
| capture | *false* | Boolean, String | Passed to the input element. Check [this documentation](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/input/file#attr-capture) for more details
| disabled | *false* | Boolean | If true, the file picker won't be triggered
| fileTypes | *false* | Array, String | A list of allowed image types. It can be either an array or comma separated. **Default:** *
| preset | *false* | String | Used when uploading unsigned pictures. It will be automatically appended to the upload request
| tag | *false* | String | Defines the component wrapper tag. **Default:** div
| uploadParams | *false* | Object | Append extra parameters to the upload request
| @picked | -- | Event | Fired when user chooses a file. It contains: file, name and url (Seeabove for more information)
| @uploaded | -- | Event | Fired after a successful. It contains the response from Cloudinary
### Slot scope
| Property | Type | Description |
| -- | -- | -- |
| file | Object | Contains the file that will be uploaded
| name | String | The name of the file selected, if available
| url | String | The file encoded in base64 for using in previews
| loading | Boolean | True if the component is currently uploading an image
| uploaded | Object | The response from Cloudinary, if available. (Same as the passed to the uploaded event)
