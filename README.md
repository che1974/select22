# select22
# Vue Select2 Wrapper Component

## Related Versions

Vue-Select2-Component is baseed on these plugins and libs(version):
- [vue(>= 2.0-release)](https://github.com/vuejs/vue)
- [jQuery](https://jquery.com/)
- [select2](https://select2.github.io/)

## How to use 
---
### Install
copy to project

```

### Use as component
``` javascript
// import Select22
import Select22 from 'select2.vue';

<template>
  <div>
            <select22
              name="clients_list"
              :options="clients"
              :dynamic_options="searchresult"
              v-model="client_id"
              @search="searchClient"
            />
  </div>
</template>
<script>
export default {

    components: {Select22},
    data() {
        return {
            client_id: '',
            searchresult: [],
            clients: {}
        }
    },
    method: {
    loadTop10Clients: function () {
      let url = window.config.params.api_server_URL + '10clients/' + this.currentUser.id;

      this.axios.get(url)
        .then(response => {
          response.data.forEach(option => {
            this.clients.push({ id: option.client_id, text: option.client.name.toString() })
          });

          // this.init();
        }, response => {
          // error callback
          this.loaded = false;

        });
    },
    searchClient: function (params) {

      let url = window.config.params.api_server_URL + 'search/client/' + params;

      this.axios.get(url)
        .then(response => {
          this.searchresult = [];
          let result = [];
          response.data.forEach(option => {
            result.push({ id: option.id, text: option.name.toString() })
          });
          this.searchresult = result;

        }, response => {
          // error callback
          // console.log(response);
          this.loaded = false;

        });
    }
    }
}
</script>
```

### Options
- `options`: `option[]`
  - select options for select2
  - `option`: `{id: key, text: value}` or `string`
- `dynamic_options`: `dynamic_option[]`
  - searchresult options for select2
- `v-model`: option value that is selected
  - `id` or `string` while multiple is disable
  - `id[]` or `string[]` while multiple is enable
- `change`
  - callback when option selected change
  - return value
  - parmas: same with `v-model`
- `select`
  - callback when option selected
  - parmas: `option`(`{id: value, text: key, selected: ifSelected}` or `string`)
- `disabled`
  - if select is disabled
- `swidth`
  - width select2 param
