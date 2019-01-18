<template>
  <select
    v-bind:name="name"
    class="form-control"
    v-bind:multiple="multiple"
  ></select>
</template>
<script>
import 'select2';
export default {
  props: {
    name: '',
    options: {
      Object
    },
    dynamic_options: {
      Object
    },
    value: null,
    multiple: {
      Boolean,
      default: false
    },
    swidth: {
      String,
      default: "100%"
    },

  },
  data () {
    return {
      select2data: []
    }
  },
  watch: {
    options: function () {
      this.formatOptions();
      this.initSelect();
    },
    dynamic_options: function () {
      this.addOptions(this.dynamic_options);
    }
  },
  mounted () {
    this.formatOptions()
    this.initSelect();
  },
  methods: {
    initSelect () {
      let vm = this
      let select = $(this.$el)
      select
        .select2({
          placeholder: 'Select',
          // theme: 'bootstrap',
          width: this.swidth,
          allowClear: true,
          data: this.select2data
        })
        .on('change', function () {
          vm.$emit('input', select.val())
        })

      select.val(this.value).trigger('change');

      //Search
      select.on('select2:open', function (e) {
        let searchBox = $('.select2-search__field');
        searchBox.on("keypress", function () {
          vm.search($(this).val());
        });
      });
    },
    formatOptions () {
      this.select2data.push({ id: '0', text: 'Select' })
      this.options.forEach(option => {
        this.select2data.push({ id: option.id, text: option.text })
      });
    },
    addOptions (data) {
      data.forEach(element => {
        this.applyOption(element);
      });
    },
    applyOption (data) {
      let vm = this
      let select = $(this.$el);

      if (select.find("option[value='" + data.id + "']").length) {
        select.val(data.id).trigger('change');
      } else {

        let option = new Option(data.text, data.id, true, true);
        select.append(option).trigger('change');

        select.trigger({
          type: 'select2:select',
          params: {
            data: data
          }
        });
      }
    },
    foo (param) {
      let select = $(this.$el)
      select.val(param).trigger('change')
    },
    search (query) {
      if (query.length >= 2) {
        this.$emit('search', query);
      }

    }
  },
  destroyed: function () {
    $(this.$el).off().select2('destroy')
  }
}
</script>

