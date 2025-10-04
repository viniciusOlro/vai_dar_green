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
            :class="{ 'inactive': !form[field].active }"
            type="range"
            min="0" 
            max="10" 
          />
          <div class="input-visibility-container">
            <strong>{{ form[field].value }}</strong>
            <input type="checkbox" v-model="form[field].active">
          </div>
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
          label: "Em quantos dos últimos 10 jogos do mandante a aposta seria ganha?",
          active: true
        },
        lastMatchesAwayGreen: {
          value: 0,
          label: "Em quantos dos últimos 10 jogos do visitante a aposta seria ganha?",
          active: true
        },
        matchBetweenBoth: {
          value: 5,
          label: "Proporção de acertos nos últimos 3-5 confrontos diretos. (Nota 0-10. Se não houver dados, usar nota neutra 5.0)",
          active: true
        },
        myPerformanceHome: {
          value: 5,
          label: "Minha % de acerto em apostas no mandante",
          active: false
        },
        myPerformanceAway: {
          value: 5,
          label: "Minha % de acerto em apostas no visitante",
          active: false
        },
        myPerformanceLeague: {
          value: 5,
          label: "Minha % de acerto em apostas na competição",
          active: false
        },
        teamsMoment: {
          value: 0,
          label: "O momento das equipes está propício para essa entrada se concretizar?",
          active: true
        },
        teamsInterest: {
          value: 0,
          label: "O interesse das equipes na partida está condizente com a entrada?",
          active: true
        }
      },
      showGreenProbability: false
    }
  },
  computed: {
    greenProbability() {
      const values = Object.values(this.form)
        .filter(item => item.active)
        .map(item => Number(item.value));
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
      text-align: center;

      input[type="range"] {
        width: 30rem;

        &.inactive {
          pointer-events: none;
          opacity: .5;
        }
      }
      .input-visibility-container {
        display: flex;
        align-items: center;
        justify-content: center;
        gap: .6rem;
        
        strong {
          color: aquamarine;
        }
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
