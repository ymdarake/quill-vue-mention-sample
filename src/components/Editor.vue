<template>
  <div>
    <quill-editor
      :content="content"
      :options="editorOption"
    >
    </quill-editor>
  </div>
</template>

<script>
// require styles
import 'quill/dist/quill.core.css'
import 'quill/dist/quill.snow.css'
import 'quill/dist/quill.bubble.css'

import { quillEditor } from 'vue-quill-editor'
import 'quill-mention';
import 'quill-mention/dist/quill.mention.min.css';


export default {
  name: 'HelloWorld',
  props: {
    msg: String
  },
  data () {
    return {
      content: '',
      editorOption: {
        modules: {
          mention: {
            allowedChars: /^[A-Za-z\sÅÄÖåäö]*$/,
            mentionDenotationChars: ["@", "#"],
            source: async function (searchTerm, renderList, mentionChar) {
              async function suggestUser (searchTerm) {
                const options = [
                  {
                    id: 1,
                    value: 'John'
                  },
                  {
                    id: 2,
                    value: 'Mark'
                  },
                  {
                    id: 3,
                    value: 'Bob'
                  },
                ];
                return options.filter((op) => op.value.includes(searchTerm));
              }
              async function suggestGroup (searchTerm) {
                const options = [
                  {
                    id: 1,
                    value: 'Sales'
                  },
                  {
                    id: 2,
                    value: 'Information Systems'
                  },
                  {
                    id: 3,
                    value: 'New Business Development'
                  }
                ];
                return options.filter((op) => op.value.includes(searchTerm));
              }
              const matchedPeople = mentionChar === '@' ? await suggestUser(searchTerm) : await suggestGroup(searchTerm);
              renderList(matchedPeople);
            }
          },
        },
      }
    }
  },
  components: {
    quillEditor
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
