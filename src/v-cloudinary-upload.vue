<template lang="pug">
component(:is="tag" @click="() => disabled || openFile()")
  input(
    ref="image"
    style="display: none"
    type="file"
    :accept="accept"
    :capture="capture"
    :disabled="disabled"
    @change="onFilePicked"
  )
  slot(v-bind="$data")
</template>

<script>
import fetch from 'node-fetch'

export default {
  name: 'VCloudinaryUpload',
  props: {
    accountName: { type: String, default: '' },
    autoUpload: { type: Boolean, default: false },
    capture: { type: [Boolean, String] },
    disabled: { type: Boolean, default: false },
    fileTypes: { type: [String, Array], default: '*' },
    preset: { type: String, default: '' },
    tag: { type: String, default: 'div' },
    uploadParams: { type: Object, default: () => ({}) },
  },
  data: () => ({
    file: '',
    loading: false,
    name: '',
    url: '',
    fileReader: null,
    uploaded: null,
  }),
  computed: {
    accept() {
      const { fileTypes } = this
      const accept =
        typeof fileTypes === 'string' ? fileTypes.split(',') : fileTypes
      return accept.map(mime => `image/${mime}`).join(', ')
    },
  },
  methods: {
    openFile() {
      this.$refs.image.click()
    },
    onFilePicked(e) {
      const files = e.target.files
      if (files[0] !== undefined) {
        this.name = files[0].name
        if (this.name.lastIndexOf('.') <= 0) {
          return
        }
        const fr = new FileReader()
        fr.readAsDataURL(files[0])
        fr.addEventListener('load', () => {
          this.url = fr.result
          this.file = files[0]
          this.fileReader = fr
          this.$emit('picked', {
            file: this.file,
            name: this.name,
            url: this.url,
            fileReader: fr,
          })
          if (this.autoUpload) this.$nextTick(() => this.upload())
        })
        return
      }
      this.name = ''
      this.file = ''
      this.url = ''
      this.fileReader = null
      this.uploaded = null
      this.$emit('picked', {
        file: this.file,
        name: this.name,
        url: this.url,
      })
    },
    upload(params = {}) {
      const { file, preset: upload_preset } = this
      const payload = Object.assign({}, { file }, params, this.params, {
        upload_preset,
      })
      const formData = new FormData()
      this.loading = true
      Object.entries(payload).forEach(([key, value]) =>
        formData.append(key, value)
      )
      return fetch(
        `https://api.cloudinary.com/v1_1/${this.accountName}/image/upload`,
        {
          method: 'POST',
          body: formData,
        }
      )
        .then(res => res.json())
        .then(body => {
          this.uploaded = body
          this.$emit('uploaded', body)
          this.loading = false
          return body
        })
    },
  },
}
</script>
