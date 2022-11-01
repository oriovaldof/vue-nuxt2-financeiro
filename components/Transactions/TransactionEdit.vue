<template>
  <div class="my-4 mt-10 space-y-4">
    <div class="grid sm:grid-cols-2 md:grid-cols-4 gap-5">
      <div>
        <AppFormLabel>Data da transação</AppFormLabel>
        <AppFormInput  v-model="localTransaction.date" type="date" />
      </div>

      <div>
        <AppFormLabel>Valor</AppFormLabel>
        <AppFormInput  v-model="localTransaction.amount" type="number" />
      </div>

      <div>
        <AppFormLabel>Descrição</AppFormLabel>
        <AppFormInput  v-model="localTransaction.description" />
      </div>

      <div>
        <AppFormLabel>Categoria</AppFormLabel>
        <AppFormSelect
        v-model="localTransaction.categoryId"
        :options="categories" />
      </div>
    </div>

    <div class="space-x-4 flex items-center justify-end">
      <a
      href=""
      class="inline-flex text-gray-700 text-sm"
      @click.stop.prevent="onCancel()"
      >
      Cancelar
    </a>

      <AppButton @click="updateTransaction">
        Editar
      </AppButton>
    </div>
  </div>
</template>
<script>
import AppButton from "~/components/Ui/AppButton";
import AppFormInput from "~/components/Ui/AppFormInput";
import AppFormLabel from "~/components/Ui/AppFormLabel";
import AppFormSelect from "~/components/Ui/AppFormSelect";

export default {
  name: "TransactionEdit",
  components: {
    AppButton,
    AppFormInput,
    AppFormLabel,
    AppFormSelect,
  },
  props: {
    transaction: {
      type: Object,
      default: () => ({}),
    },
  },
  data: () => ({
    localTransaction:{},
    categories:[]
  }),
  async fetch(){
    // this.localTransaction = JSON.parse(JSON.stringify(this.transaction)),
    this.localTransaction = {
        date: this.transaction.date,
        amount: this.transaction.amount,
        description: this.transaction.description,
        categoryId: this.transaction.category.id,
      },
    this.categories = await this.$store.dispatch("categories/getCategories")
  },
  methods: {
    updateTransaction() {

      this.$store.dispatch("transactions/updateTransaction", {
        id: this.transaction.id,
        data: this.localTransaction
      }).then((response)=>{
        this.$emit('update',{
          ...response,
          category: this.categories.find(i => i.id == this.localTransaction.categoryId)
        });
        this.onCancel();
      });
    },
    onCancel(){
      this.$emit('cancel')
    }
  },
};
</script>
