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

## Contributing
```
npm install
```
### Compiles and hot-reloads for development
```
npm run serve
```
### Compiles and minifies for production
```
npm run build
```
### Lints and fixes files
```
npm run lint
```

### Customize configuration
See [Configuration Reference](https://cli.vuejs.org/config/).