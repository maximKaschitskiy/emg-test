
## task for EMG

### Задание
Необходимо написать сервис с авторизацией и кастомной формой ввода. В ней должны содержаться следующие элементы:
Поле ввода текста на 2 строки (не допускаются цифры)
Загрузка картинки (ограничение в 5 Мб)
Кнопка Добавить
Кнопка Добавить сохраняет данные в базу данных.
К серверу должен быть реализовать endpoint запрос GET, который получает в ответ последний добавленный элемент из базы данных в виде json – текст и ссылка на картинку.


### Solution
##### Backend:
Server based on Node.js and Express library. 3001 port. Stored in MongoDB. Uploaded images are accessed via direct link. Secure routes require validation with the Bearer token in the header's Authorization parameter.
  
  How to run:
  At first, run Node.JS server:
  
      npm run dev
  
  Then, run MongoDB daemon:
  
      mongod
  
###### Routes:
  
  - GET "/cards" - public. Returns the last "card"-entry from the database. If the base is empty, the appropriate message is returned.
  
    Response example:
    
        {
          "descriptionFirst": "description",
          "descriptionSecond": "description",
          "file": "http://localhost:3001/file-1650018026407.jpg",
        }
	
  - POST "/signup" - public. Takes login and password to register a new user account. Accepted values ​​are validated for a repeated login field, the number and validity of characters. Accounts are stored in the database, the password is hashed.
  
    Request example:
    
	    {
          "login": "sipus2006@yandex.ru",
          "password": "qwerty12345"
        }
	
  - POST "/signin" - public. Takes the login and password for the login of an already registered user. Accepted values ​​are validated for a repeated login field, the number and validity of characters. Returns a Bearer token.
	
	Request example:
	
	    {
          "login": "sipus2006@yandex.ru",
          "password": "qwerty12345"
        }
	
    Response example:
    
	    {
	      "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJfaWQiOiI2MjU5NDY3ZmY1MzFhMGI2YWZlZmMyMmQiLCJpYXQiOjE2NTAwMTc5NTUsImV4cCI6MTY1MDYyMjc1NX0.yNM8JnhNGqZZwjCmYmFGf9MIfg4RVk2SaTmLvfhSK04"
        }
	
  - POST "/cards" - private. Takes FormData object containing two fields with string and a file. The image with the changed file name is saved in the "uploads" directory. The server shares images by direct route. Returns a JSON object with a description and the uploaded file link. Taked values ​​are validated for a uniqueness login field, the number and validity of characters, the size and format of the file.
  
    Response example:
    
	    {
          "descriptionFirst": "description",
          "descriptionSecond": "description",
          "file": "http://localhost:3001/file-1650018026407.jpg",
        }
	
##### Frontend:
There is page on React. Protected routes are using the Bearer token in the header's Authorization parameter. The token is stored in the browser's localStorage if the login form are successful submit.

  Launch:
  
      npm run start

###### Routes:
  
 - "/" - protected. Displays the form for sending a "card" to the server. The fields are validated. If there is no valid token in localStorage, a redirect to the authorization page occurs.

 - "/sign-in" - public. Displays the login form. The fields are validated.

 - "/sign-up" - public. Displays the registration form. The fields are validated. Upon successful registration, you are redirected to the login page.

##### Technology:
- API: node.js, mongodb, mongoose, joi, bcrypt, multer
- Frontend: react
