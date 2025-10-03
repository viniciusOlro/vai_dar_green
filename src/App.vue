<template>
  <div>
    <header>
      <h1>Vai dar Green?</h1>
    </header>
    <main>
      <form>
        <div 
          v-for="field in Object.keys(form)"
          class="input-container"
        >
          <label :for="field">
            {{ form[field].label }}
          </label>
          <input
            v-model="form[field].value" 
            :id="field" 
            :name="field" 
            type="range"
            min="0" 
            max="10" 
          />
          <strong>{{ form[field].value }}</strong>
        </div>
      </form>
      <button @click="showGreenProbability = !showGreenProbability">
        {{ showGreenProbability ? 'Ocultar' : 'Mostrar' }}
      </button>
      <h4 v-if="showGreenProbability">{{ greenProbability }}</h4>
    </main>
  </div>
</template>

<script>
export default {
  name: 'App',
  data() {
    return {
      form: {
        lastMatchesHomeGreen: {
          value: 0,
          label: "Em quantos dos últimos 10 jogos do mandante a aposta seria ganha?"
        },
        lastMatchesAwayGreen: {
          value: 0,
          label: "Em quantos dos últimos 10 jogos do visitante a aposta seria ganha?"
        },
        matchBetweenBoth: {
          value: 5,
          label: "Proporção de acertos nos últimos 3-5 confrontos diretos. (Nota 0-10. Se não houver dados, usar nota neutra 5.0)"
        },
        myPerformanceHome: {
          value: 5,
          label: "Minha % de acerto em apostas no mandante"
        },
        myPerformanceAway: {
          value: 5,
          label: "Minha % de acerto em apostas no visitante"
        },
        myPerformanceLeague: {
          value: 5,
          label: "Minha % de acerto em apostas na competição"
        },
        teamsMoment: {
          value: 0,
          label: "O momento das equipes está propício para essa entrada se concretizar?"
        },
        teamsInterest: {
          value: 0,
          label: "O interesse das equipes na partida está condizente com a entrada?"
        }
      },
      showGreenProbability: false
    }
  },
  computed: {
    greenProbability() {
      const values = Object.values(this.form).map(item => Number(item.value));
      return (values.length > 0 ? values.reduce((sum, v) => sum + v, 0) / values.length : 0).toFixed(2);
    }
  }
}
</script>

<style lang="scss">
header {
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 3.2rem;
  h1 {
    color: green;
  }
}

main {
  padding-bottom: 3.2rem;
  display: flex;
  align-items: center;
  justify-content: center;
  flex-direction: column;
  gap: 4.8rem;
  
  form {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 2.4rem;

    .input-container {
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center;

      input {
        width: 30rem;
      }

      strong {
        color: aquamarine;
      }
    }
  }

  button {
    border: none;
    outline: none;
    background: aquamarine;
    padding: 1.6rem;
    border-radius: .3rem;
    font-weight: bold;
    color: black;
    cursor: pointer;
  }
}

@media (max-width: 768px) {
  main {
    form {
      display: flex;
      flex-direction: column;
    }
  }
}
</style>
