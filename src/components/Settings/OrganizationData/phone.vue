<template lang="pug">
  .phoneComponent
    .row.q-pb-sm(v-for="(item, index) in arrayLength" :key="index")
      .col
        q-input(
          ref="tel"
          v-model="organization.phones[index]"
          type="tel"
          mask=" +# (###) ### - ####"
          unmasked-value
          hint="Mask: +# (###) ### - ####"
          :rules="[val => val.length === 11 || 'Номер должен содержать 11 цифр']"
          outlined
          dense
        )
          template(#after)
            q-icon.cursor-pointer(name="delete" @click="deletePhone(index)" title="Удалить телефон")
    .phoneBtn.col
      q-btn.q-mt-sm.bg-primary.text-white(label="Добавить телефон" @click="addPhone" outlined style="width: 100%;")
</template>

<script>
export default {
  name: 'phones',
  props: {
    organization: Object
  },
  data () {
    return {
      arrayLength: 0
    }
  },
  watch: {
    'organization.phones.length' () {
      this.arrayLength = this.organization.phones.length
    }
  },
  created () {
    if (!this.organization.hasOwnProperty('phones')) return
    this.arrayLength = this.organization.phones.length
  },
  methods: {
    addPhone () {
      if (this.arrayLength < 5) {
        this.arrayLength++
      } else {
        this.showNotif('Не более 5 телефонных номеров!', 'orange')
      }
    },
    deletePhone (index) {
      const result = this.organization.phones.splice(index, 1)
      if (!result.length) this.arrayLength--
    },
    showNotif (msg, clr = 'purple') {
      this.$q.notify({
        message: msg,
        color: clr
      })
    }
  }
}
</script>

<style scoped>

</style>
