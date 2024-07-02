# INFOTECH⚙️

## В ходе выполнения тестового задания были совершены следующие действия:
<ul>
<li>  Изучена форма авторизации, размещённая на странице https://stellantis.autocrm.ru/  </li> 
<li>  Написан чек-лист с проверками для формы авторизации  </li> 
<li>  Составлены 2 баг-репорта  </li> 
<li>  Составлены позитивный и негативный тест-кейс  </li> 
<li>  Написан пример запроса API  </li> 
</ul>

### С чек-листом, тест-кейсами и баг-репортами вы можете ознакомиться по [ссылке](https://docs.google.com/spreadsheets/d/1HmIiAKB9zJ63P_5F99xf3bdtGdGDEc-pAzVIvsNJbM0/edit?usp=sharing)

### Пример запроса API для авторизации на сайте [stellantis.autocrm.ru](https://stellantis.autocrm.ru/login)

#### Примечание: этот код пригоден для исполнения в консоли браузера; CSRF-токен извлекается автоматически; на случай, если браузер не поддерживает `fetch`, я использовал `XMLHttpRequest`


``` js
// Функция для отправки POST-запроса на авторизацию
async function login() {
  try {
    // Создаем XMLHttpRequest для получения страницы авторизации
    const xhr = new XMLHttpRequest();
    xhr.open('GET', 'https://stellantis.autocrm.ru/login', true);

    // Обработчик загрузки страницы авторизации
    xhr.onload = function () {
      if (xhr.status >= 200 && xhr.status < 300) {
        // Получаем HTML-код страницы
        const html = xhr.responseText;

        // Создаем временный элемент для парсинга HTML
        const parser = new DOMParser();
        const doc = parser.parseFromString(html, 'text/html');

        // Извлекаем CSRF-токен из полученной страницы
        const csrfToken = doc.querySelector('input[name="_csrf"]').value;

        // Создаем новый XMLHttpRequest для отправки данных авторизации
        const xhrLogin = new XMLHttpRequest();
        xhrLogin.open('POST', 'https://stellantis.autocrm.ru/login', true);

        // Настройка заголовков запроса
        xhrLogin.setRequestHeader('Content-Type', 'application/x-www-form-urlencoded');

        // Обработчик загрузки ответа на запрос авторизации
        xhrLogin.onload = function () {
          if (xhrLogin.status >= 200 && xhrLogin.status < 300) {
            console.log('Авторизация успешна:', xhrLogin.responseText);
          } else {
            console.error('Ошибка авторизации:', xhrLogin.status);
          }
        };

        // Данные для отправки (форма авторизации с CSRF-токеном)
        const formData = `LoginForm[email]=your_email@example.com&LoginForm[password]=your_password&_csrf=${encodeURIComponent(csrfToken)}`;

        // Отправляем данные на сервер
        xhrLogin.send(formData);
      } else {
        console.error('Не удалось получить страницу авторизации:', xhr.status);
      }
    };

    // Отправляем GET-запрос для получения страницы авторизации
    xhr.send();
  } catch (error) {
    console.error('Произошла ошибка:', error);
  }
}

// Вызываем функцию для выполнения запроса на авторизацию
login();

