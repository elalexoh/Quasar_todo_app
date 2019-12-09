<template>
  <div class="q-pa-md q-gutter-sm">
    <q-editor
      v-model="editor"
      :definitions="{
        save: {
          tip: 'Guardar task',
          icon: 'save',
          label: 'Save',
          handler: saveWork
        },
      }"
      :toolbar="[
        ['bold', 'italic', 'strike', 'underline'],
        ['upload', 'save']
      ]"
    />
    <q-card class="row" flat bordered v-for="(item, index) in tasks" :key="index">
      <q-card-section :class="item.estado ? 'tachar': ''" class="col" v-html="item.texto" />
      <q-btn flat color="blue" @click="item.estado = !item.estado">Acción</q-btn>
      <q-btn flat color="red" @click="eliminar(index)">Eliminar</q-btn>
    </q-card>
    <div class="flex flex-center" v-if="tasks.length == 0">
      <h6>Sin notas</h6>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      editor: "",
      tasks: [
        { texto: "Tarea #1", estado: false }
        //{ texto: "Tarea #2", estado: true },
        //{ texto: "Tarea #3", estado: false }
      ]
    };
  },
  methods: {
    saveWork() {
      this.tasks.push({
        texto: this.editor,
        estado: false
      });
      this.$q.notify({
        message: "Saved your text to local storage",
        color: "green-4",
        textColor: "white",
        icon: "cloud_done"
      });
    },
    eliminar(index) {
      this.$q
        .dialog({
          title: "Acción Peligrosa",
          message: "Realmente quieres eliminar la task?",
          cancel: true,
          persistent: true
        })
        .onOk(() => {
          this.tasks.splice(index, 1);
        });
    }
  }
};
</script>
<style lang="scss">
.tachar {
  text-decoration: line-through;
}
</style>