<template>

<div class="buttons">
<button @click="isVisibleAlltickets = !isVisibleAlltickets; isVisibleVolh = false"><h1>Все заявки</h1></button>
<button @click="isVisibleVolh = !isVisibleVolh; isVisibleAlltickets = false"><h1>КПО Волхонка</h1></button>
</div>  

<br><br>

<div v-if="isVisibleAlltickets">
  <div class="tickets-list">
    <div v-for="ticket in sliceArray" :key="ticket.id">
      <div class="priority">{{ ticket.pryority }}</div>
      <div class="ticket">
        
        <button @click="toggleTicketInfo(ticket.id)">
        {{ticket.id}} - {{ticket.title}}</button>
          <div v-if="ticket.isOpen">

            <div class="info2">
            <div class="info1">
              Тут что-то должно быть.
            </div>
              Статус заявки: {{ticket.status}} <br>
              Адрес: {{ticket.area}} <br>
              Закрепленный сотрудник: {{ticket.worker}}
            </div>
        </div>
      </div>
    </div>
  </div>
  {{currentPage}} / {{countPages}}
  <div class="changePageButtons">
  <button @click="currentPage--" :disabled="currentPage === 1">Назад</button>
  <button @click="currentPage++" :disabled="currentPage * countTicketsOnPage >= tickets.length">Вперёд</button>
  </div>
</div>



<div v-if="isVisibleVolh">
  <div v-for="ticket in tickets" :key="ticket.id">
    <div v-if="ticket.area === 'КПО Волхонка'">
      Номер заявки: {{ticket.id}} <br>
      Описание заявки: {{ticket.title}} <br>
      Статус заявки: {{ticket.status}} <br>
      Адрес: {{ticket.area}} <br>
      Закрепленный сотрудник: {{ticket.worker}} <br><br>
    </div>
  </div>
</div>

</template>




<script setup>
import { ref, computed } from 'vue'
const isVisibleAlltickets = ref(true)
const isVisibleVolh = ref(false)

const tickets = ref([
  { id: 123654, pryority: 5, isOpen: false, title: 'Не включается компьютер', status: 'open', area: 'БЦ Кондратьевский', worker: 'Дроботенко Егор' },
  { id: 123655, pryority: 5,isOpen: false, title: 'Нет интернета', status: 'open', area: 'КПО Волхонка', worker: 'Кулик Дмитрий' },
  { id: 123656, pryority: 4,isOpen: false, title: 'Заменить картридж', status: 'open', area: 'КПО Островский', worker: 'Нагнибеда Владислав' },
  { id: 123657, pryority: 4,isOpen: false, title: 'Не включается 1С', status: 'open', area: 'БЦ Арсенальный', worker: 'Григорьев Максим' },
  { id: 123658, pryority: 5,isOpen: false, title: 'Проблема в почте, прошу решить в кратчайшие сроки. Не могу отправить письмо заявителю', status: 'open', area: 'БЦ Фернан Леже', worker: 'Морозов Алексей' },
  { id: 123659, pryority: 3,isOpen: false, title: 'Не работает Глонасс', status: 'open', area: 'БЦ Фернан Леже', worker: 'Морозов Алексей' },
])

const countTicketsOnPage = 10
const countPages = computed(() => Math.ceil(tickets.value.length / countTicketsOnPage))

const currentPage = ref(1);
const sliceArray = computed(() =>{
let start = (currentPage.value - 1) * countTicketsOnPage
let end = start + countTicketsOnPage
return tickets.value.slice(start, end)
})

// Функция по проверке раскрыта ли заявка
function toggleTicketInfo(ticketId) {
  const ticket = tickets.value.find(item => item.id === ticketId)
  ticket.isOpen = !ticket.isOpen
}

</script>

<style scoped>
.buttons {
  display: flex;
  gap: 10px;
  text-align: center;
  width: 30%;
}

button {
  text-align: left;
  width: 100%;
}

.changePageButtons {
  text-align: center;
  justify-content: center;
  gap: 10px;
  display: flex;
  width: 15%;
}

.tickets-list {
  min-height: 400px;
}

.ticket {
  align-items: stretch;
  border: 2px solid #020101;
  border-radius: 5px;
  margin-bottom: 10px;
  max-width: 650px;
}

.priority {
  width: 40px;
  display: flex;
  border-radius: 5px;
  align-items: center;
  justify-content: center;
  font-weight: bold;
  background: #c8b6b6;
}

.info2 {
  width: 100%;

  text-align: right;
  background: linear-gradient(to bottom, #9c8282, #bbb3b3);
}

.info1{

  justify-content: left;
  background: linear-gradient(to bottom, #281010, #bbb3b3);
}

</style>
