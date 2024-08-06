<template>
  <div class="background">
    <div class="row-1">
      <div class="part-one">
        <div class="filter">
          <q-select label="Ano" v-model="year.value" rounded :options="year.options"
            style="width: 110px; border-radius: 36px !important;" outlined dense>
            <template v-slot:prepend>
              <UIcon name="i-fluent-calendar-12-regular" class="text-gray-7" size="20px"></UIcon>
            </template>
          </q-select>
          <q-select label="Medição" v-model="month.value" rounded :options="month.options"
            style="width: 140px; border-radius: 36px !important;" outlined dense>
            <template v-slot:prepend>
              <UIcon name="i-fluent-calendar-date-20-regular" class="text-gray-7" size="25px"></UIcon>
            </template>
          </q-select>
          <q-select label="Selecionar Período" v-model="period.value" rounded :options="period.options"
            style="width: 207px; border-radius: 36px !important;" outlined dense @click="openCalendar">
            <template v-slot:prepend>
              <UIcon name="i-fluent-calendar-date-20-regular" class="text-gray-7" size="25px"></UIcon>
            </template>
            <!-- ABA DE MENU - CALENDARIO         --------------------------------------------- -->
            <q-menu style="width: 650px;">
              <div class="flex q-gutter-md">
                <q-card>
                  <q-list bordered class="periodo">
                    <q-item v-for="(period, index) in periods" :key="index">
                      <q-item-section>
                        <q-radio v-model="selectedPeriod" :val="period.value" :label="period.label" />
                      </q-item-section>
                    </q-item>
                  </q-list>
                </q-card>
                
                  <div class="data">
                    <div class="data1" >
                      <q-date v-model="startDate" flat class="custom-q-date" minimal range @input="handleStartDateChange" :range="rangeDates" />
                    </div>
                  </div>
  
                  <div class="back">
                    <div class="inputs-buttons">
                      <q-input v-model="formattedStartDate" mask="##.##.####" filled class="dateStart" :dense="dense" />
                      <q-input v-model="formattedEndDate" mask="##.##.####" filled class="dateEnd" :dense="dense" />
                      <q-btn @click="onCancel" class="cancelar" text-color="#1A3B47">Cancelar</q-btn>
                      <q-btn @click="onApply" class="aplicar" text-color="white">Aplicar</q-btn>
                    </div>
                  </div>
                
              </div>
            </q-menu>




          </q-select>
          <q-select label="Classificação" v-model="classification.value" rounded :options="classification.options"
            style="width: 170px; border-radius: 36px !important;" outlined dense>
            <template v-slot:prepend>

              <svg xmlns="http://www.w3.org/2000/svg" width="1em" height="1em" viewBox="0 0 20 20">
                <path fill="currentColor"
                  d="M6.494 8.075c.045-.295.339-.575.754-.575s.709.28.754.575a.5.5 0 1 0 .988-.15C8.863 7.09 8.104 6.5 7.248 6.5s-1.615.59-1.742 1.425a.5.5 0 1 0 .988.15m6.254-.575c-.415 0-.708.28-.754.575a.5.5 0 0 1-.988-.15c.127-.835.886-1.425 1.742-1.425s1.615.59 1.742 1.425a.5.5 0 0 1-.988.15c-.046-.295-.339-.575-.754-.575M5.495 10a.5.5 0 0 0-.497.549C5.236 12.992 7.154 15 9.995 15s4.76-2.008 4.998-4.451a.5.5 0 0 0-.498-.549zm4.5 4c-2.081 0-3.516-1.303-3.912-3h7.825c-.397 1.697-1.831 3-3.913 3M10 2a8 8 0 1 0 0 16a8 8 0 0 0 0-16m-7 8a7 7 0 1 1 14 0a7 7 0 0 1-14 0">
                </path>
              </svg>
            </template>

            <template v-slot:option="scope">
              <q-item v-bind="scope.itemProps">
                <q-item-section>
                  <q-item-label class="flex q-gutter-sm items-center">
                    <img :src="`/${scope.opt.path}`" alt="">
                    <div>{{ scope.opt.label }}</div>
                  </q-item-label>
                </q-item-section>
              </q-item>
            </template>
          </q-select>
        </div>
        <hr class="divider" />
        <div class="selected-filters">
          <button v-if="year.value" class="filter-tag-ano">
            {{ year.value }}
            <span class="del cursor-pointer" @click="year.value = ''"></span>
          </button>
          <button v-if="month.value" class="filter-tag-mes">
            {{ month.value }}
            <span class="del cursor-pointer" @click="month.value = ''"></span>
          </button>
          <button v-if="period.value" class="filter-tag-data flex">
            {{ period.value }}
            <span class="del cursor-pointer" @click="period.value = ''"></span>
          </button>
          <!-- <button v-if="classification.value" class="filter-tag-Classificação flex">
                      {{ classification.value }}
                      <span class="del cursor-pointer" @click="classification.value = ''"></span>
                  </button> -->
          <button class="clear-button" @click="clearFilters">Limpar tudo</button>
        </div>
      </div>
      <div class="part-two">
        <div class="card positive1">
          <div class="text1">Pesquisas positivas</div>
          <div class="text2">ITSM</div>
          <div class="text3">22</div>
        </div>
        <div class="card negative1">
          <div class="text1">Pesquisas negativas</div>
          <div class="text2">ITSM</div>
          <div class="text3">05</div>
        </div>
        <div class="card positive2">
          <div class="text1">Pesquisas positivas</div>
          <div class="text2">CHAT</div>
          <div class="text3">01</div>
        </div>
        <div class="card negative2">
          <div class="text1">Pesquisas negativas</div>
          <div class="text2">CHAT</div>
          <div class="text3">00</div>
        </div>
      </div>
      <div class="part-three">
        <img src="/public/avaliar.png" alt="Avaliar" />
      </div>
    </div>
    <div class="part-four">
      <q-markup-table>
        <thead>
          <tr>
            <th class="text-top">
              Indicador<button class="vector5"></button>
            </th>
            <th class="text-top">
              Valor admissível<button class="vector5"></button>
            </th>
            <th class="text-top">
              Sua equipe<button class="vector5"></button>
            </th>
            <th class="text-top">
              Seus resultados<button class="vector5"></button>
            </th>
            <th class="text-top">
              Classificação<button class="vector5"></button>
            </th>
            <th class="text-top">
              Resumo<button class="vector5"></button>
            </th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="indicator in indicatorsComp" :key="indicator.id">
            <td class="text-left">{{ indicator.Indicador }}</td>
            <td class="text-right1">{{ indicator['Valor admissível'] }}</td>
            <td class="text-right1">{{ indicator['Sua equipe'] }}</td>
            <td class="text-right2">{{ indicator['Seus resultados'] }}</td>
            <td class="text-right3">
              <img :src="getMoodImage(indicator.Classificação)" alt="Mood Image" class="mood-icon" />
            </td>
            <td class="text-right1">{{ indicator.Resumo }}</td>
          </tr>
        </tbody>
      </q-markup-table>
    </div>
    <div class="part-five">
      <div v-if="showTooltip" class="tooltip">Ver todos</div>
      <q-btn round class="circular-btn" @click="toggleTooltip">
        <UIcon size="32px" class="icon-content" name="i-fluent-chevron-down-20-regular"></UIcon>
      </q-btn>
      <q-separator size="4px" class="line"></q-separator>
    </div>

  </div>
</template>

<script>
import indicadores from "../mocks/indicadores.json";
import cardCalendar from "../components/cardCalendar.vue";
import { ref, computed } from 'vue'


export default {
  name: "MeuComponente",
  components: {
    cardCalendar,
  },
  data() {
    return {
      showTooltip: false,
      showCalendar: false,
      year: {
        value: "",
        options: [2021, 2022, 2023, 2024],
      },
      month: {
        value: "",
        options: [
          "Janeiro",
          "Fevereiro",
          "Março",
          "Abril",
          "Maio",
          "Junho",
          "Julho",
          "Agosto",
          "Setembro",
          "Outubro",
          "Novembro",
          "Dezembro",
        ],
      },
      period: {
        value: "",
        options: [],
      },
      classification: {
        value: "",
        options: [
          {
            label: "Bom",
            path: "bom.svg",
            value: "bom"
          },
          {
            label: "Muito bom",
            path: "muitobom.svg",
            value: "muitobom"
          },
          {
            label: "Regular",
            path: "regular.svg",
            value: "regular"
          },
          {
            label: "Ruim",
            path: "ruim.svg",
            value: "ruim"
          },
          {
            label: "Muito ruim",
            path: "muitoruim.svg",
            value: "muitoruim"
          },
        ],
      },
      indicators: indicadores,
      indicatorsComp: indicadores,
    };
  },
  computed: {
    yearValue() {
      return this.year.value;
    },
    monthValue() {
      return this.month.value;
    },
    periodValue() {
      return this.period.value;
    },
    classificationValue() {
      return this.classification.value;
    },
  },
  watch: {
    yearValue(newYear) {
      this.filterIndicators();
    },
    monthValue(newMonth) {
      this.filterIndicators();
    },
    periodValue(newPeriod) {
      this.filterIndicators();
    },
    classificationValue(newClassification) {
      this.filterIndicators();
    },
  },
  methods: {
    toggleTooltip() {
      this.showTooltip = !this.showTooltip;
    },
    openCalendar() {
      this.showCalendar = true;
    },
    indicatorsByYear(year) {
      return this.indicators.filter(
        (item) => new Date(item.created_at).getFullYear() === year
      );
    },
    indicatorsByMonth(month) {
      const monthIndex = this.month.options.indexOf(month);
      return this.indicators.filter(
        (item) => new Date(item.created_at).getMonth() === monthIndex
      );
    },
    indicatorsByPeriod(period) {
      // Adicione a lógica de filtragem por período aqui, se necessário
      return this.indicators.filter(
        (item) => item.period === period // Exemplo de filtragem por período
      );
    },
    indicatorsByClassification(classification) {
      return this.indicators.filter(
        (item) => item.Classificação === classification
      );
    },
    filterIndicators() {
      // Começa com todos os indicadores
      let filteredIndicators = this.indicators;

      // Aplica cada filtro, refinando a lista a cada passo
      if (this.year.value) {
        filteredIndicators = filteredIndicators.filter(
          (item) => new Date(item.created_at).getFullYear() === this.year.value
        );
      }

      if (this.month.value) {
        const monthIndex = this.month.options.indexOf(this.month.value);
        filteredIndicators = filteredIndicators.filter(
          (item) => new Date(item.created_at).getMonth() === monthIndex
        );
      }

      if (this.period.value) {
        // Adapte este filtro conforme necessário
        filteredIndicators = filteredIndicators.filter(
          (item) => item.period === this.period.value // Exemplo de filtragem por período
        );
      }

      if (this.classification.value) {
        filteredIndicators = filteredIndicators.filter(
          (item) => {
            console.log(item)
            return item.Classificação === this.classification.value.value
          }
        );
        console.log(this.classification.value.value, filteredIndicators)
      }

      // Atualiza a lista de indicadores filtrados
      this.indicatorsComp = filteredIndicators;
    },
    clearFilters() {
      this.year.value = "";
      this.month.value = "";
      this.period.value = "";
      this.classification.value = "";
      this.indicatorsComp = this.indicators;
    },
    getMoodImage(classification) {
      const moodImages = {
        muitobom: "muitobom.svg",
        bom: "bom.svg",
        regular: "regular.svg",
        ruim: "ruim.svg",
        muitoruim: "muitoruim.svg",
      };
      return moodImages[classification] || "bom.svg";
    },
  },

  //PARTE DE MENU CALENDARIO -----------------------------
  setup() {
    const periods = [
      { label: 'Hoje', value: 'today' },
      { label: 'Essa semana', value: 'thisWeek' },
      { label: 'Esse mês', value: 'thisMonth' },
      { label: 'Esse ano', value: 'thisYear' },
      { label: 'Selecionar', value: 'select' }
    ]
    const selectedPeriod = ref(null)
    const startDate = ref(null)
    const endDate = ref(null)

    // Computed property to handle the range
    const rangeDates = computed(() => {
      if (startDate.value && endDate.value) {
        return [startDate.value, endDate.value]
      }
      return []
    })

    const formattedStartDate = computed(() => formatDate(startDate.value))
    const formattedEndDate = computed(() => formatDate(endDate.value))

    const dense = ref(true)

    function formatDate(date) {
      if (!date) return ''
      const d = new Date(date)
      return `${d.getDate().toString().padStart(2, '0')}.${(d.getMonth() + 1).toString().padStart(2, '0')}.${d.getFullYear()}`
    }

    function handleStartDateChange(newStartDate) {
      startDate.value = newStartDate
      if (endDate.value && new Date(startDate.value) > new Date(endDate.value)) {
        endDate.value = startDate.value
      }
    }

    function handleEndDateChange(newEndDate) {
      endDate.value = newEndDate
      if (startDate.value && new Date(endDate.value) < new Date(startDate.value)) {
        startDate.value = endDate.value
      }
    }

    function onCancel() {
      startDate.value = null
      endDate.value = null
    }

    function onApply() {
      console.log('Data Inicial:', formattedStartDate.value)
      console.log('Data Final:', formattedEndDate.value)
    }

    return {
      periods,
      selectedPeriod,
      startDate,
      endDate,
      formattedStartDate,
      formattedEndDate,
      dense,
      onCancel,
      onApply,
      rangeDates,
      handleStartDateChange,
      handleEndDateChange
    }
  }
};
</script>


<style>
.custom-dialog .q-dialog__inner {
  width: 799px;
  height: 362px;
  max-width: 90vw;
  max-height: 90vh;
  position: fixed;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
}

.background {
  display: flex;
  flex-direction: column;
  align-items: center;
  height: 780px;
  width: 1756px;
  background-color: #FFFFFF;
  border: 2px solid #CCCCCC;
}

.row-1 {
  display: flex;
  width: 100%;
  margin-bottom: -60px;
  margin-top: 10px;
  position: fixed; 
  top: 0; 
  margin-top: 80px;
  left: 0; 
  width: 100%; 
  position: fixed;
  z-index: 1000; 
}
/* 
display: flex;
  flex-direction: column;
  align-items: center;
  margin-top: 10px;
  position: fixed; 
  top: 0; 
  margin-top: 760px;
  left: 0; 
  width: 100%; 
  background-color: white;
  z-index: 1000;  */

.vector {
  display: inline-block;
  width: 14px;
  height: 14px;
  background-image: url(/public/Vector.png);
  background-size: cover;
  margin-right: 5px;
}

.vector2 {
  display: inline-block;
  width: 18px;
  height: 18px;
  background-image: url(/public/Vector2.png);
  background-size: cover;
  margin-right: 5px;
  padding: 4px 5px;
  margin-left: 3.2px;
}

.vector3 {
  display: inline-block;
  width: 14px;
  height: 14px;
  background-image: url(/public/Vector3.png);
  background-size: cover;
  margin-right: 9px;
}

.vector4 {
  display: inline-block;
  width: 15px;
  height: 15px;
  background-image: url(/public/Vector4.png);
  background-size: cover;
  margin-right: 8px;
}

.vector5 {
  display: inline-block;
  width: 18px;
  height: 18px;
  background-image: url(/public/Vector5.png);
  background-size: cover;
  margin-right: 8px;
  padding: 4px 5px;
  margin-left: 3.2px;
}

/* .del {
  display: inline-block;
  width: 18px;
  height: 18px;
  padding: 4px 5px;
  background-image: url(/public/del.png);
  background-size: cover;
} */

.part-one,
.part-two,
.part-three {
  display: flex;
  align-items: center;
  background-color: #FFFFFF;
  margin-left: 45px;
  margin-right: 60px;
  margin-top: 25px;
  
}

.part-one {
  height: 98.5px;
  width: 650px;
  flex-wrap: wrap;
  /* background-color: #1A3B47; */
}

.part-two {
  height: 64px;
  width: 590px;
  flex-wrap: wrap;
  justify-content: space-around;
}

.part-three {
  height: 72px;
  width: 211px;
}

.part-four {
  width: 1666px;
  /* display: flex; */
  flex-direction: column;
  align-items: center;
  margin-top: 920px;
  position: fixed; 
  top: 0; 
  margin-top: 210px;
  left: 0; 
  width: 100%; 
  background-color: white;
  z-index: 1000;  
}

.filter-button-ano {
  background-color: #FFFFFF;
  border: 1px solid #CCCCCC;
  border-radius: 36px;
  padding: 5px 9px;
  width: 92px;
  height: 32px;
  font-family: "SohoGothicPro-Regular";
}

.filter-button-medicao {
  background-color: #FFFFFF;
  border: 1px solid #CCCCCC;
  border-radius: 36px;
  padding: 5px 9px;
  width: 122px;
  height: 32px;
  font-family: "SohoGothicPro-Regular";
}

.filter-button-periodo {
  background-color: #FFFFFF;
  border: 1px solid #CCCCCC;
  border-radius: 36px;
  padding: 3px 9px;
  width: 193px;
  height: 32px;
  font-family: "SohoGothicPro-Regular";
}

.filter-button-Classificação {
  background-color: #FFFFFF;
  border: 1px solid #CCCCCC;
  border-radius: 36px;
  padding: 3px 9px;
  width: 155px;
  height: 32px;
  font-family: "SohoGothicPro-Regular";
}

.divider {
  width: 100%;
  height: 10px;
  border-top: 1px solid #CCD1D2;
  align-items: center;
}

.selected-filters {
  width: 543px;
  height: 32px;
  font-weight: 600;
  display: flex;
  flex-wrap: wrap;
  align-items: center;
}

.selected-filters span {
  margin-right: 16px;
}

.filter {
  height: 35px;
  margin-top: -10px;
  gap: 16px;
  font-weight: 600;
  display: flex;
  align-items: center;
}


.filter-tag-ano,
.filter-tag-mes,
.filter-tag-data {
  display: flex; 
  align-items: center; 
  justify-content: center; 
  background-color: #FFFFFF;
  border: 1px solid #CCCCCC;
  border-radius: 36px;
  height: 32px;
  font-family: "SohoGothicPro-Regular";
  margin-right: 10px;
  padding: 0 6px;
  text-align: center; 
}

.filter-tag-ano {
  width: 90px; 
}

.filter-tag-mes {
  width: 120px;
}

.filter-tag-data {
  width: auto; 
}

.del {
  cursor: pointer;
  font-weight: bold;
  color: #CCCCCC;
  margin-left: 5px; 
}

.remove {
  margin-left: 5px;
  cursor: pointer;
  font-weight: bold;
  color: #CCCCCC;
}

.clear-button {
  background-color: #e4e3e3;
  border-radius: 30px;
  width: 115px;
  height: 32px;
  padding: 5px 10px;
  cursor: pointer;
  font-weight: 700;
  font-family: "SohoGothicPro-Regular";
  margin-left: 10px; 
}

.clear-button:hover {
  background-color: #e1e1e1;
}

.card {
  display: flex;
  flex-direction: column;
  align-items: center;
  border-radius: 10px;
  width: 150px;
  height: 80px;
  color: white;
  size: 12px;
  font-family: "SohoGothicPro-Regular";
}

.text1 {
  display: flex;
  flex-direction: column;
  align-items: center;
  width: 150px;
  color: white;
  font-weight: 100;
  size: 20px;
  font-family: "SohoGothicPro-Regular";
}

.text2 {
  size: 14px;
  font-weight: 500;
  line-height: 16px;
  align-items: center;
  width: 120px;
  height: 16px;
  font-family: "SohoGothicPro-Regular";
  gap: 300px;
}

.text3 {
  size: 16px;
  font-weight: 600;
  line-height: 16px;
  align-items: center;
  width: 120px;
  height: 16px;
  font-family: "SohoGothicPro-Regular";
}

.positive1 {
  background-color: #1A3B47;
  width: 139px;
  height: 64px;
}

.negative1 {
  background-color: #E88A8A;
  width: 144px;
  height: 64px;
}

.positive2 {
  background-color: #1A3B47;
  width: 139px;
  height: 64px;
}

.negative2 {
  background-color: #E88A8A;
  width: 144px;
  height: 64px;
}

/* Quadro de resultados */
.text-left {
  width: 80px;
  /* Ajuste conforme necessário */
  padding: 4px 8px;
  /* Reduzindo o padding */
  font-weight: 700;
  font-size: 18px;
  /* Corrigido para font-size */
  line-height: 24px;
  letter-spacing: -2%;
  color: #1A3B47;
  font-family: "SohoGothicPro-Regular";
}

.text-right1 {
  width: 100px;
  font-family: "SohoGothicPro-Regular";
  font-weight: 400;
  font-size: 16px;
  line-height: 24px;
  letter-spacing: -2%;
  color: #1A3B47;
  text-align: center;
}

.text-right2 {
  width: 100px;
  font-family: "SohoGothicPro-Regular";
  font-weight: 700;
  font-size: 16px;
  line-height: 24px;
  letter-spacing: -2%;
  color: #77DD22;
  text-align: center;
}

.text-right3 {
  width: 100px;
  line-height: 24px;
  align-items: center;
}

.text-top {
  font-family: "SohoGothicPro-Regular";
  font-weight: 500;
  font-size: 16px;
  line-height: 24px;
  letter-spacing: -2%;
  color: #A3AED0;
  padding: 4px 8px;
  width: auto;
}

.part-five {
  display: flex;
  flex-direction: column;
  align-items: center;
  margin-top: 10px;
  position: fixed; 
  top: 0; 
  margin-top: 760px;
  left: 0; 
  width: 100%; 
  background-color: white;
  z-index: 1000; 
}

.circular-btn {
  /* width: 35px;
height: 35px;
border-radius: 20px; */
  position: relative;
  z-index: 1;
  background-color: #1A3B47;
  color: #FFFFFF;
}

.tooltip {
  background-color: #1A3B47;
  color: white;
  width: 77px;
  height: 48px;
  border-radius: 8px;
  position: absolute;
  top: 760px;
  /* Ajuste conforme necessário */
  font-size: 14px;
  font-family: "SohoGothicPro-Regular";
  z-index: 2;
  line-height: 16px;
  display: flex;
  /* Adicionado para flexbox */
  align-items: center;
  /* Centralização vertical */
  justify-content: center;
  /* Centralização horizontal */
}

.tooltip::after {
  content: '';
  position: absolute;
  bottom: -6px;
  left: 50%;
  transform: translateX(-50%);
  border-width: 6px;
  border-style: solid;
  border-color: #1A3B47 transparent transparent transparent;
}

.line {
  width: 550px;
  position: relative;
  top: -23px;
  /* Ajuste conforme necessário para alinhar com o botão */
  background: rgb(26, 59, 71);
  background: radial-gradient(circle, rgba(26, 59, 71, 1) 0%, rgba(255, 255, 255, 1) 91%);
}

.mood-icon {
  width: 24px;
  height: 24px;
}

::v-deep .q-field__native {
  padding-left: 32px;
}

.q-field {
  width: 275px;
}

/* PARTE DO MENU DO CALENDARIO ------------------*/
.custom-q-date {
    /* Define a largura e a altura desejadas */
    width: 400px;
    height: 300px;
    /* Ajuste o tamanho da fonte para o texto dentro do componente */
    font-size: 16px;
  }

.background {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100%;
  gap: 80px;
  font-family: "SohoGothicPro-Regular";
  font-weight: 600;
}

/* .container {
  
} */

.periodo {
  width: 190px;
  height: 362px;
  align-items: center;
}

/* .period-container {
  width: 238px;
} */

.data {
  display: flex;
  justify-content: space-between;
  width: 50%;
}

.data1 {
  margin-top: 120px;
  margin-left: 20px;
  width: 450px;
  height: 62px;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  border-radius: 10px;
}

.back {
  margin-top: 310px;
  margin-left: -350px;
  width: 450px;
  height: 62px;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  border-radius: 10px;
}

.inputs-buttons {
  display: flex;
  align-items: center;
  
}

/* .separator {
  margin: 0 5px;
} */

.q-input {
  width: 80px;
  height: 32px;
  font-size: 12px;
}

.dateStart,
.dateEnd {
  border-radius: 20px;
  border: 2px solid #1A3B47;
  height: 32px;
  font-size: 12px;
}

.dateStart {
  margin-right: 10px;
  width: 93px;
}

.dateEnd {
  margin-right: -25px;
  width: 97px;
}

.cancelar {
  margin-right: 10px;
  margin-left: 50px;
  background-color: #EFEFEF;
  font-family: "Roboto";
  border-radius: 30px;
  font-weight: 700;
  text-transform: none;
}

.aplicar {
  background-color: #1A3B47;
  font-family: "Roboto";
  border-radius: 30px;
  font-weight: 500;
  text-transform: none;
}
</style>