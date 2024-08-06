<template>
  <div style="width: 650px;">
    <div class="flex q-gutter-md">
      <q-card>
        <q-list bordered class="periodo">
          <q-item v-for="(period, index) in periods" :key="index">
            <q-item-section>
              <q-radio
                v-model="selectedPeriod"
                :val="period.value"
                :label="period.label"
                @update:model-value="handlePeriodChange"
              />
            </q-item-section>
          </q-item>
        </q-list>
      </q-card>

      <div class="data">
        <div class="data1">
          <q-date
            v-model="rangeDates"
            flat
            class="custom-q-date"
            minimal
            range
            @input="handleDateChange"
          />
        </div>
      </div>

      <div class="back">
        <div class="inputs-buttons">
          <q-input
            v-model="formattedStartDate"
            mask="##.##.####"
            filled
            class="dateStart"
            :dense="dense"
          />
          <q-input
            v-model="formattedEndDate"
            mask="##.##.####"
            filled
            class="dateEnd"
            :dense="dense"
          />
          <q-btn @click="onCancel" class="cancelar" text-color="#1A3B47">
            Cancelar
          </q-btn>
          <q-btn @click="onApply" class="aplicar" text-color="white">
            Aplicar
          </q-btn>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { ref, computed, watch } from 'vue'

export default {
  setup() {
    const periods = [
      { label: 'Hoje', value: 'today' },
      { label: 'Essa semana', value: 'thisWeek' },
      { label: 'Esse mês', value: 'thisMonth' },
      { label: 'Esse ano', value: 'thisYear' },
      { label: 'Selecionar', value: 'select' },
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

    // Watcher for selectedPeriod
    watch(selectedPeriod, (newPeriod) => {
  const today = new Date();

  if (newPeriod === "today") {
    // Set today's date using local time
    const todayDate = new Date(
      today.getFullYear(),
      today.getMonth(),
      today.getDate()
    )
      .toISOString()
      .split("T")[0];
    startDate.value = todayDate;
    endDate.value = todayDate;
  } else if (newPeriod === "thisWeek") {
    const dayOfWeek = today.getDay();
    const firstDayOfWeek = new Date(today);
    const lastDayOfWeek = new Date(today);

    firstDayOfWeek.setDate(today.getDate() - dayOfWeek + 1); // Começa na segunda-feira
    lastDayOfWeek.setDate(today.getDate() + (7 - dayOfWeek)); // Termina no domingo

    // Caso seja domingo (dayOfWeek === 0)
    if (dayOfWeek === 0) {
      firstDayOfWeek.setDate(today.getDate() - 6);
      lastDayOfWeek.setDate(today.getDate());
    }

    startDate.value = firstDayOfWeek.toISOString().split("T")[0];
    endDate.value = lastDayOfWeek.toISOString().split("T")[0];
  } else if (newPeriod === "thisMonth") {
    const firstDayOfMonth = new Date(today.getFullYear(), today.getMonth(), 1); // Primeiro dia do mês
    const lastDayOfMonth = new Date(today.getFullYear(), today.getMonth() + 1, 0); // Último dia do mês

    startDate.value = firstDayOfMonth.toISOString().split("T")[0];
    endDate.value = lastDayOfMonth.toISOString().split("T")[0];
  } else if (newPeriod === "thisYear") {
    const firstDayOfYear = new Date(today.getFullYear(), 0, 1); // 1º de janeiro do ano atual
    const lastDayOfYear = new Date(today.getFullYear(), 11, 31); // 31 de dezembro do ano atual

    startDate.value = firstDayOfYear.toISOString().split("T")[0];
    endDate.value = lastDayOfYear.toISOString().split("T")[0];
  }
});


    function formatDate(date) {
      if (!date) return ''
      const d = new Date(date)
      return `${d.getDate().toString().padStart(2, '0')}.${(d.getMonth() + 1)
        .toString()
        .padStart(2, '0')}.${d.getFullYear()}`
    }

    function handleDateChange(dates) {
      if (dates.length > 0) {
        startDate.value = dates[0]
        endDate.value = dates[1]
      }
    }

    function handlePeriodChange(periodValue) {
      selectedPeriod.value = periodValue
    }

    function onCancel() {
      selectedPeriod.value = null
      startDate.value = null
      endDate.value = null
      rangeDates.value = []
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
      handleDateChange,
      handlePeriodChange,
    }
  },
}
</script>
  
  <style>
  .custom-q-date {
    width: 400px;
    height: 300px;
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
  
  .periodo {
    width: 190px;
    height: 362px;
    align-items: center;
  }
  
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
  