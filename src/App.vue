<script setup>
import { reactive } from 'vue';


const data = reactive ({
  sender: '',
  billTo: '',
  shipTo: '',
  invoiceNumber: '',
  date: '',
  dueDate: '',
  additionalNote: '',
  items: [{
    description: '',
    qunatity: '',
    rate: '',
    discount: '',
    amount: '',
  }],
  notes: '',
  terms: '',
  subTotal: '',
  tax: '',
  total: ''
})

const addMoreItem = () => {
  data.items.push({
    description: '',
    qunatity: '',
    rate: '',
    discount: '',
    amount: '',
  })
}

const getSubTotal = () => {
  let subTotal = 0
  data.items.forEach(item=> {
    subTotal += item.amount
  })
  data.subTotal = subTotal
  return subTotal
}

const getTotal = () => {
  const tax = data.subTotal * data.tax / 100
  data.total = data.subTotal + tax
  return data.total
}

const deleteItem = (index) => {
      data.items.splice(index, 1)
  }




</script>

<template>
  <section class="bg-gray-300 w-full h-full pb-20 pt-20">
    <div
      class="container mx-auto border-4 border-black-700 bg-white pt-5 px-10 pb-5"
    >
      <div class="flex justify-between">
        <div class="flex flex-col space-y-5 w-1/2s">
          <div class=" ">
            <img
              class="w-40"
              src="//www.shutterstock.com/image-vector/invoice-typographic-stamp-sign-badge-600w-1027820257.jpg"
              alt=""
            />
          </div>
          <p class="mt-5">Sender</p>
          <textarea v-model="data.sender" name="" id="" cols="30" rows="2"></textarea>
          <div class="flex space-x-2">
            <div class="flex flex-col">
              <span>Bill to</span>
              <textarea v-model="data.billTo" name="" id="" cols="30" rows="2"></textarea>
            </div>
            <div class="flex flex-col">
              <span>Ship to</span>
              <textarea v-model="data.shipTo" name="" id="" cols="30" rows="2"></textarea>
            </div>
          </div>
        </div>
        <div class="flex flex-col w-1/2 items-end">
          <h1 class="mt-12 text-4xl uppercase text-right mb-5">Invoice</h1>
          <input
            v-model="data.invoiceNumber"
            class="w-[200px] text-right"
            type="text"
            placeholder="Invoice Number"
          />
          <div class="mt-10 flex-y-5 text-right space-y-3 w-full">
            <p>
              <span>Date</span>
              <input v-model="data.date" class="ml-2 w-[200px]" />
            </p>
            <p>
              <span>Due Date</span>
              <input v-model="data.dueDate" class="ml-2 w-[200px]" />
            </p>
            <p>
              <span>Additional Note</span>
              <input v-model="data.additionalNote" class="ml-2 w-[200px]" type="text" />
            </p>
          </div>
        </div>
      </div>

      <div class="mt-20">
        <table class="table-auto w-full">
          <tr class="bg-gray-800 text-left text-white">
            <th class="p-2 pl-5">Item</th>
            <th class="p-2">Quantity</th>
            <th class="p-2">Rate</th>
            <th class="p-2">Discount</th>
            <th class="p-2 w-[200px] text-right pr-5">Amount</th>
            <th class="p-2 w-[100px] text-right pr-5">Action</th>
          </tr>
          <tr  v-for="(item, index) in data.items" :key="index" >
            <td class="py-1">
              <input
                v-model="item.description"
                class="w-full pl-5"
                type="text"
                placeholder="Description"
              />
            </td>
            <td class="">
              <input v-model="item.qunatity" class="w-full" type="number" placeholder="Quantity" />
            </td>
            <td class="">
              <input v-model="item.rate" class="w-full" type="number" placeholder="Rate" />
            </td>
            <td class="">
              <input v-model="item.discount" class="w-full" type="number" placeholder="Discount" />
            </td>
            <td class="py-1 pr-5 text-right text-gray-800">
             $ {{ item.amount = (item.qunatity * item.rate) - item.discount }}
            </td>
            <td class="text-center text-red-700">
              <i @click="deleteItem(index)" class="fas fa-trash-alt cursor-pointer text-2xl"></i>
            </td>
          </tr>
        </table>
        <button
          class="mt-5 bg-green-600 hover:bg-green-700 text-white font-bold py-2 px-4 rounded"
          @click="addMoreItem()"
        >
          Add More
        </button>
      </div>

      <div class="mt-[150px]">
        <div class="flex justify-between">
          <div class="flex flex-col space-y-5 w-1/2">
            <span>Notes</span>
            <textarea v-model="data.notes" name="" id="" cols="30" rows="2"></textarea>
            <span>Terms</span>
            <textarea v-model="data.terms" name="" id="" cols="30" rows="2"></textarea>
          </div>
          <div class="flex flex-col w-1/2 items-end">
            <div class="mt-10 flex-y-5 text-right space-y-3 w-full">
              <p>
                <span>Subtotal</span>
                <input
                  readonly
                  class="focus:ring-0 focus:ring-offset-0 text-right ml-2 pr-4 w-[200px]"
                  placeholder="Subtotal"
                  :value="getSubTotal()"
                />
              </p>
              <p>
                <span>Tax</span>
                <input v-model="data.tax" type="number" class="tax text-right w-[200px] ml-2" />
              </p>
              <p>
                <span>Total</span>
                <input
                  :value="getTotal()"
                  readonly
                  class="focus:ring-0 focus:ring-offset-0 text-right ml-2 pr-4 w-[200px]"
                  placeholder="Total"
                />
              </p>
            </div>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>

<style scoped></style>
