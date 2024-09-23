<template>
  <div>
    <h1>Cadastro de Paciente</h1>
    <form @submit.prevent="submitForm">
      <!-- Dados do Paciente -->
      <div>
        <label>Código do paciente:</label>
        <input v-model="patient.code" required />
      </div>

      <!-- Atividades -->
      <h2>Atividades</h2>
      <div v-for="(activity, index) in patient.activities" :key="index">
        <label>Descrição da Atividade:</label>
        <input v-model="activity.description" placeholder="Descrição" required />
        <label>Nota:</label>
        <input v-model="activity.score" type="number" min="0" max="10" placeholder="Nota (0-10)" required />
        <button type="button" @click="removeActivity(index)">Remover</button>
      </div>

      <button type="button" @click="addActivity">Adicionar Atividade</button>

      <br /><br />
      <button type="submit">Enviar</button>
    </form>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data() {
    return {
      patient: {
        code: "", // Código do Paciente
        activities: [], // Atividades
      },
    };
  },
  methods: {
    addActivity() {
      this.patient.activities.push({ description: "", score: null });
    },
    removeActivity(index) {
      this.patient.activities.splice(index, 1);
    },
    async submitForm() {
      // Capturar o dia e horário atuais
      const currentDateTime = new Date();
      const date = currentDateTime.toLocaleDateString(); // Data no formato local (dd/mm/yyyy)

      try {
        // Loop através de cada atividade e enviá-la individualmente
        for (const activity of this.patient.activities) {
          const dataToSend = {
            codigo_do_paciente: this.patient.code, // Corrigido para usar this.patient.code
            descricaoAtividade: activity.description, // Descrição da Atividade
            notaAtividade: activity.score, // Nota da Atividade
            dataEnvio: date, // Data de Envio
          };

          const response = await axios.post(
            "https://api.sheetmonkey.io/form/keboAXgkeWL77ZR39TKRLb",
            dataToSend
          );

          console.log(response.data);
        }

        alert("Dados enviados com sucesso!");
      } catch (error) {
        console.error("Erro ao enviar os dados", error);
        alert("Erro ao enviar os dados");
      }
    },
  },
};
</script>

<style scoped>
form {
  display: flex;
  flex-direction: column;
}

label {
  margin-top: 10px;
}

input,
select {
  margin: 5px 0 10px;
}

button {
  margin-top: 10px;
}
</style>
