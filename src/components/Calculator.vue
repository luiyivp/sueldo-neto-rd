<template>
  <div
    class="max-w-sm p-6 bg-white border border-gray-200 rounded-lg shadow dark:bg-gray-800 dark:border-gray-700"
  >
    <h1
      class="mb-2 text-2xl pb-3 font-bold tracking-tight text-gray-900 dark:text-white"
    >
      Calculadora de Sueldo Neto
    </h1>
    <h2 class="text-justify mb-3 font-normal text-gray-700 dark:text-gray-400">
      Utiliza esta calculadora para conocer cual es tu sueldo neto mensual
      después de descontar las retenciones de ley de la República Dominicana.
    </h2>
    <p
      class="mb-3 text-sm pb-2 font-extralight text-gray-600 dark:text-gray-400"
    >
      Los campos obligatorios son aquellos que tiene un asterisco (*) en el
      título
    </p>
    <form class="max-w-sm mx-auto" onsubmit="return false">
      <div class="mb-5">
        <label
          for="salary"
          class="block mb-2 text-sm font-medium text-gray-900 dark:text-white"
          >Ingresa El Sueldo Bruto Mensual *</label
        >
        <div class="flex">
          <span
            class="inline-flex items-center px-3 text-sm text-gray-900 bg-gray-200 border border-e-0 border-gray-300 rounded-s-md dark:bg-gray-600 dark:text-gray-400 dark:border-gray-600"
          >
            RD$
          </span>
          <input
            type="number"
            id="salary"
            v-model="salary"
            class="rounded-none bg-gray-50 border border-gray-300 text-gray-900 text-sm rounded-e-lg rounded-lg focus:ring-indigo-500 focus:border-indigo-500 block w-full p-2.5 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-indigo-500 dark:focus:border-indigo-500"
            placeholder="25000"
            required
          />
        </div>
      </div>

      <button
        type="summit"
        @click="calculate()"
        class="text-white bg-indigo-700 hover:bg-indigo-800 focus:ring-4 focus:outline-none focus:ring-indigo-300 font-medium rounded-lg text-medium w-full sm:w-auto px-5 py-3 text-center dark:bg-indigo-600 dark:hover:bg-indigo-700 dark:focus:ring-indigo-800"
      >
        Calcular
      </button>
    </form>
    <h5
      class="mb-2 text-xl pt-6 text-center font-medium tracking-tight text-gray-900 dark:text-white"
    >
      Resultado del cálculo de tu Sueldo
    </h5>
    <div class="relative overflow-x-auto">
      <table
        class="w-full text-sm text-left rtl:text-right text-gray-500 dark:text-gray-400"
      >
        <tbody>
          <tr class="bg-white border-b dark:bg-gray-800 dark:border-gray-700">
            <th
              scope="row"
              class="py-2 font-normal text-gray-900 whitespace-nowrap dark:text-white"
            >
              Sueldo Bruto:
            </th>
            <td class="py-2 text-right">{{ salaryValue }}</td>
          </tr>
          <tr class="bg-white border-b dark:bg-gray-800 dark:border-gray-700">
            <th
              scope="row"
              class="py-2 font-normal text-gray-900 whitespace-nowrap dark:text-white"
            >
              SFS (3.04%)
            </th>
            <td class="py-2 text-right">{{ sfsValue }}</td>
          </tr>
          <tr class="bg-white border-b dark:bg-gray-800 dark:border-gray-700">
            <th
              scope="row"
              class="py-2 font-normal text-gray-900 whitespace-nowrap dark:text-white"
            >
              AFP (2.87%)
            </th>
            <td class="py-2 text-right">{{ afpValue }}</td>
          </tr>
          <tr class="bg-white border-b dark:bg-gray-800 dark:border-gray-700">
            <th
              scope="row"
              class="py-2 font-normal text-gray-900 whitespace-nowrap dark:text-white"
            >
              Impuesto Sobre la Renta:
            </th>
            <td class="py-2 text-right">{{ isrValue }}</td>
          </tr>
          <tr class="bg-white dark:bg-gray-800">
            <th
              scope="row"
              class="py-2 text-base text-indigo-700 whitespace-nowrap dark:text-white"
            >
              Sueldo Neto:
            </th>
            <td class="py-2 font-bold text-indigo-700 text-right">
              {{ netIncomeValue }}
            </td>
          </tr>
        </tbody>
      </table>
    </div>
    <p
      class="mb-2 text-xs pb-2 mt-2 text-justify font-extralight text-gray-500 dark:text-gray-400"
    >
      * Tener en cuenta que esta calculadora de salario neto considera
      únicamente las prestaciones sociales mínimas establecidas por la ley de la
      República Dominicana. Los resultados obtenidos en este ejercicio son con
      fines informativos y es responsabilidad del usuario hacer un uso adecuado
      y diligente de la información proporcionada.
    </p>
    <a
      href="https://github.com/luiyivp/sueldo-neto-rd"
      target="_blank"
      rel="noopener noreferrer"
      class="flex items-center mb-0 mt-2 sm:mb-0 space-x-3 rtl:space-x-reverse"
    >
      <img
        src="https://cdn-icons-png.flaticon.com/512/25/25231.png"
        class="h-8"
        alt="GitHub Logo"
      />
    </a>
  </div>
</template>

<script>
export default {
  data: () => ({
    salary: "",
    salaryValue: "---",
    sfs: "",
    sfsValue: "---",
    afp: "",
    afpValue: "---",
    isr: "",
    isrValue: "---",
    netIncome: "",
    netIncomeValue: "---",
  }),
  methods: {
    calculate() {
      if (this.salary) {
        this.sfs = this.salary * 0.0304;
        this.afp = this.salary * 0.0287;
        this.isr = this.calculateIncomeTax(this.salary);
        this.netIncome = this.salary - this.sfs - this.afp - this.isr;

        this.salaryValue = this.formatCurrency(this.salary);
        this.sfsValue = this.formatCurrency(this.sfs);
        this.afpValue = this.formatCurrency(this.afp);
        this.isrValue = this.formatCurrency(this.isr);
        this.netIncomeValue = this.formatCurrency(this.netIncome);
      }
    },
    calculateIncomeTax(salary) {
      let annualSalary = salary * 12;

      if (annualSalary > 867123) {
        return ((annualSalary - 867123.01) * 0.25 + 79776) / 12;
      } else if (annualSalary > 624329) {
        return ((annualSalary - 624329.01) * 0.2 + 31216) / 12;
      } else if (annualSalary > 416220) {
        return ((annualSalary - 416220.01) * 0.15) / 12;
      } else {
        return 0;
      }
    },
    formatCurrency(value) {
      return value.toLocaleString(`es-DO`, {
        style: "currency",
        currency: "DOP",
      });
    },
  },
};
</script>

<style>
/* Hide the spin buttons in WebKit browsers */
input::-webkit-outer-spin-button,
input::-webkit-inner-spin-button {
  -webkit-appearance: none;
  margin: 0;
}

/* Hide spin buttons in Firefox */
input[type="number"] {
  -moz-appearance: textfield;
}
</style>