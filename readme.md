# quill-video-extend-module

## install

```bash
npm install quill-video-extend-module --save
```

## Usage


```html
<quill-editor 
  ref="myTextEditor" 
  v-model="editorContent" 
  :options="editorOption">
</quill-editor>
```

```js
import { VideoExtend, QuillVideoWatch } from './quill-video-extend-module'
import { quillEditor, Quill } from 'vue-quill-editor'
import 'quill/dist/quill.core.css'
import 'quill/dist/quill.snow.css'
import 'quill/dist/quill.bubble.css'

Quill.register('modules/VideoExtend', VideoExtend)

export default {
  props: {
    content: {
      required: true
    },
    placeholder: {
      default: '...'
    },
    videoUploadUrl: {
      default: ''
    },
    token: {
      required: false
    },
    toolbar: {
      required: false
    }
  },
  data() {
    const container = [
      [{ header: [1, 2, 3, 4, 5, 6, false] }],
      ['bold', 'italic', 'underline', 'strike'], // toggled buttons

      [{ list: 'ordered' }, { list: 'bullet' }],
      [{ script: 'sub' }, { script: 'super' }], // superscript/subscript
      [{ indent: '-1' }, { indent: '+1' }], // outdent/indent

      [{ color: [] }, { background: [] }], // dropdown with defaults from theme

      ['image', 'video', 'clean']
    ]
    return {
      editorContent: this.content,
      editorOption: {
        placeholder: this.placeholder,
        modules: {
          toolbar: {
            container: container,
            handlers: {
              'video': function() {
                QuillVideoWatch.emit(this.quill.id)
              }
            }
          },
          history: {
            delay: 1000,
            maxStack: 50,
            userOnly: false
          },
          VideoExtend: {
            loading: true,
            name: 'video',
            action: this.videoUploadUrl,
            headers: (xhr) => {
              // set custom token(optional)
              xhr.setRequestHeader('token', this.token)
            },
            response: (res) => {
              // video uploaded path
              // custom your own
              return process.env.BASE_API + res.data.path
            }
          }
        }
      },
    }
  }
}

```
