<template>
  <div class="container-fluid overflow-hidden">
    <div class="row justify-content-evenly m-2">
      <!-- Левая часть экрана, место для ввода пользователем данных -->
      <div class="col">
        <div class="p-3 myBorder">
          <!--          Поле ввода статуса-->
          <input-simple-select
              initial_value="Новый"
              :options="statusArray"
              spanValue="Статус"
              @getData="inputStatus"
          />
          <!--          Поле ввода ФИО-->
          <input-group-of3-elements
              @inputFIO="inputFIO"
          />
          <!--          Поле ввода города-->
          <input-with-use-api-city
              @getData="inputCity"
              @getIdCity="inputCityId"
          />
          <!--          Поле ввода фото-->
          <input-simple
              spanValue="Ссылка на фото"
              placeholderValue="URL"
              ariaLabelValue="URL"
              @getData="inputFhoto"
          />
          <!--          Поле ввода профессии-->
          <input-with-regex
              spanValue="Профессия"
              placeholderValue="Профессия"
              ariaLabelValue="Профессия"
              errorMessage="Профессия не соответствует формату"
              regexPattern='^[А-Я][А-Яа-я ]{2,70}'
              @getData="inputProfession"
          />
          <!--          Поле ввода телефона-->
          <input-with-regex
              spanValue="Телефон"
              placeholderValue="Номер телефона"
              ariaLabelValue="Телефон"
              errorMessage="Номер не соответствует формату"
              regexPattern='^((\+7|7|8)+([0-9]){10})$'
              @getData="inputPhoneNumber"
          />
          <!--          Поле ввода имейла-->
          <input-with-regex
              spanValue="EMAIL"
              placeholderValue="example@mail.com"
              ariaLabelValue="email"
              errorMessage="EMAIL не соответствует формату"
              regexPattern='^[-\w.]+@([A-z0-9][-A-z0-9]+\.)+[A-z]{2,4}$'
              @getData="inputEmail"
          />
          <!--          Поле ввода др-->
          <input-birthdate-with-regex
              @getData="inputBirthdate"
          />
          <!--          Поле ввода образования-->
          <input-education-with-use-api
              :id_city="id_city"
              @returnAllEducation="inputEducation"
              @getSelect="inputSelectEducation"
          />
          <!--          Поле ввода зарплаты-->
          <input-simple-range
              spanValue="Желаемая ЗП (₽)"
              placeholderValue="ЗП"
              ariaLabelValue="ЗП"
              minValue="15000"
              maxValue="250000"
              stepValue="1000"
              @getData="inputSalary"
          />
          <!--          Поле ввода ключевых навыков-->
          <textarea-with-regex
              spanValue="Ключевые навыки"
              errorMessage="Используйте буквы латинского алфавита. А так же пробелы и . , ! ? : ;"
              regexPattern='^[а-яА-Я .,!?:;]{0,200}'
              @getData="inputKeySkills"
          />
          <!--          Поле ввода данных о себе-->
          <textarea-with-regex
              spanValue="О себе"
              errorMessage="Используйте буквы латинского алфавита. А так же пробелы и . , ! ? : ;"
              regexPattern='^[а-яА-Я .,!?:;]{0,200}'
              @getData="inputAboutMe"
          />
        </div>
      </div>
      <!-- Правая часть экрана, отображения данных введенным пользователем -->
      <div class="col">
        <div class="p-3 myBorder" style="display: flex;">
          <div class="me-3"
               style="min-height: 300px; min-width: 250px; max-height: 300px; max-width: 250px; background-color: white;">
            <img :src="photo" class="img-fluid" style="display: flex; justify-content: center; color: #727A82;"
                 alt="Тут могло быть ваше фото">
          </div>
          <div>
            <div>Статус: {{ status }}</div>
            <br>
            <div>ФИО: {{ FIO }}</div>
            <br>
            <div>Город: {{ city }} {{ id_city }}</div>
            <br>
            <div>Профессия: {{ profession }}</div>
            <br>
            <div>Номер телефона: {{ phoneNumber }}</div>
            <br>
            <div>EMAIL: {{ email }}</div>
            <br>
            <div>Дата рождения: {{ birthdate }}</div>
            <br>
            <div>Уровень образования: {{ education }}</div>
            <div v-if="toggleEducation" class="mt-1">
              <div
                  v-for="value in allEducation" v-bind:key="value.id"
                  class="myBorderEducation container-fluid"
              >
                Учебное заведение: {{ value.educational_institution }} <br>
                Факультет: {{ value.faculty }} <br>
                Специализация: {{ value.specialization }} <br>
                Год окончания: {{ value.year_of_ending }}
              </div>
            </div>
            <br>
            <div>Желаемая зарплата: {{ salary }}₽</div>
            <br>
            <div>Ключевые навыки: {{ key_skills }}</div>
            <br>
            <div>О себе: {{ about_me }}</div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import inputGroupOf3Elements from '@/components/input-group-of-3-elements.vue'
import inputWithRegex from '@/components/input-with-regex.vue'
import textareaWithRegex from "@/components/textarea-with-regex.vue";
import InputBirthdateWithRegex from "@/components/input-birthdate-with-regex.vue";
import InputSimple from "@/components/input-simple.vue";
import InputSimpleRange from "@/components/input-simple-range.vue";
import InputSimpleSelect from "@/components/input-simple-select.vue";
import InputWithUseApiCity from "@/components/input-with-use-api-city.vue";
import InputEducationWithUseApi from "@/components/input-education-with-use-api.vue";

export default {
  name: 'App',
  components: {
    InputEducationWithUseApi,
    InputWithUseApiCity,
    InputSimpleSelect,
    InputSimpleRange,
    InputSimple,
    InputBirthdateWithRegex,
    inputGroupOf3Elements,
    inputWithRegex,
    textareaWithRegex
  },
  data() {
    return {
      FIO: '',
      photo: '',
      profession: "",
      phoneNumber: "",
      email: "",
      birthdate: "",
      education: "",
      toggleEducation: false,
      salary: "",
      key_skills: "",
      about_me: "",
      status: "",
      city: "",
      id_city: "",
      statusArray: [
        {id: 1, value: "Назначено собеседование"},
        {id: 2, value: "Принят"},
        {id: 3, value: "Отказ"},
      ],
      allEducation: [],
    }
  },
  methods: {
    // Сохранения данных о фио
    inputFIO(FIO) {
      this.FIO = FIO;
    },
    // Сохранения данных о фото
    inputFhoto(photo) {
      this.photo = photo;
    },
    // Сохранения данных о профессии
    inputProfession(profession) {
      this.profession = profession;
    },
    // Сохранения данных о номере телефона
    inputPhoneNumber(phoneNumber) {
      this.phoneNumber = phoneNumber;
    },
    // Сохранения данных о имейле
    inputEmail(email) {
      this.email = email;
    },
    // Сохранения данных о ключевых скилах
    inputKeySkills(key_skills) {
      this.key_skills = key_skills;
    },
    // Сохранения данных о "о себе"
    inputAboutMe(about_me) {
      this.about_me = about_me;
    },
    // Сохранения данных о др
    inputBirthdate(birthdate) {
      this.birthdate = birthdate;
    },
    // Сохранения данных о зарплате
    inputSalary(salary) {
      this.salary = salary;
    },
    // Сохранения данных о статусе
    inputStatus(status) {
      this.status = status;
    },
    // Сохранения данных о городе
    inputCity(city) {
      this.city = city;
    },
    // Сохранения данных о айди города
    inputCityId(id_city) {
      this.id_city = id_city;
    },
    //  Обработка ввода данных об образовании
    inputEducation(allEducation) {
      this.allEducation = allEducation;
    },
    // Функция изменения переключателя при вводе уровня образования
    inputSelectEducation(select) {
      this.education = select;
      if (this.education === "Среднее") {
        this.toggleEducation = false;
      } else {
        this.toggleEducation = true;
      }
    }
  }
}
</script>

<style>

div.myBorder {
  border-color: #DFDEDD;
  border-style: solid;
  border-width: 5px;
}

body {
  min-height: 100%;
  background-image: url("../src/assets/theme.jpeg");
  background-repeat: no-repeat;
  background-size: cover;
  background-position: top center;
}

.customAlert {
  padding: 0;
  width: fit-content;
  margin-bottom: unset;
}

.col {
  min-height: 500px;
}

div.myBorderEducation {
  border-color: #EAD5B6;
  border-style: solid;
  border-width: 5px;
}
</style>
