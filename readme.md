# Code Challenge

# Visual Thinking API

## Practica 5 semana 4 LaunchX Backend Node Mission

Tiempo para resolver el challenge: 2 horas 10 minutos

Formato readme: 40 minutos

### 1. Dependencias.

#### Node - Proyecto

Proyecto en Node v16.14.2

Creación del proyecto con el comando  (generando el archivo package.json):

- npm init

#### Git - Gestión de Versiones

El proyecto cuenta con gestión de versiones con Git. Creando un repositorio local y un reposotorio Remoto en GitHub.

Cada cambio en el proyecto se puede revisar en los [commits](https://github.com/Chaylander/Code_Challenge/commits/master).

Nota: En el proyecto tenemos .gitignore para no versionar el folder node_models.

#### Jest - Pruebas Unitarias

El proyecto tiene instalado Jest para la realización de pruebas unitarias.

Para su instalacion y configuración, se corrieron los siguientes comandos:

- npm install --save-dev jest

Configuración en package.json para agregar el script para correr jest:

- "test": "node ./node_modules/jest/bin/jest.js"

Para ejecutar todas las pruebas unitarias:

- npm test

#### Express - API

El proyeto tiene istalado express para crear la API solicitada.

Comando para instalar express en el proyecto:

- npm install express --save

Se agregó al package.json la siguiente linea, en scripts

`"server": "node ./libs/server.js"`

Para correr el server con npm, de la siguiente manera

* npm run serve

#### Linter

Permite revisar el cógigo para darle estilo mediante reglas ya establecidas previamente.

Comando para su instalación:

- npm install eslint --save-dev

Comando para la configuracion, general el archivo .eslintrc.js en el proyecto.

- npm init @eslint/config

Agregar reglas en [.eslintrc.js](https://github.com/Chaylander/Code_Challenge/blob/master/.eslintrc.js) y scripts de linter en package.json.

 Reglas de linter:

```
   "rules": {
        indent: ["error", 4],
        "linebreak-style": ["error", "unix"],
        quotes: ["error", "double"],
        semi: ["error", "always"]
    }
```

 Scripts de linter en package.json

```
 "linter": "node ./node_modules/eslint/bin/eslint.js .",
 "linter-fix": "node ./node_modules/eslint/bin/eslint.js . --fix" 
```

#### GitHub Actions - Automatización de Pruebas

Dentro del proyecto se crea el archivo test.yml en la ubicación .github/workflows para automatizar las pruebas unitarias en GitHub Actions cuando se realiza un push al repositorio remoto. En la parte de las actions nos notificará en caso de que alguna prueba no haya pasado. Funciona con la versión actual de jest por lo que no es necesario utilizar una anterior.

[test.yml](https://github.com/Chaylander/Code_Challenge/blob/master/.github/workflows/test.yml)

### 2. Diseño de los componentes.

Se utilizaron 3 componentes

| Componente                                                                                                        | Función                                                                                                                 |
| ----------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------ |
| [Reader.js](https://github.com/Chaylander/Code_Challenge/blob/master/lib/utils/Reader.js)                            | Clase encargada de la lectura del archivo que contiene la db ej formato .json mediante el método reservado readJsonFile |
| [StudentServices.js](https://github.com/Chaylander/Code_Challenge/blob/master/lib/Services/StudentServices.js)       | Clase que permite el filtrado de los estudiantes, recibe como parametro el .json leído por el Reader                    |
| [StudentConroller.js](https://github.com/Chaylander/Code_Challenge/blob/master/lib/Controllers/StudentController.js) | Clase que utiliza a la clase StudentServices y Reader para conectar con la API                                           |

#### Métodos de las clases

* Student Services

|                                          Requerimientos                                          | Método estático                                        | Parámetros                                                                                                                        |
| :-----------------------------------------------------------------------------------------------: | -------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------- |
|                      1. Consultar todos los estudiantes con todos sus campos                      | getAllStudents(students)                                 | students es el array del .json leído en el Reader                                                                                 |
| 2. consultar los emails de todos los estudiantes que tengan certificación `haveCertification`. | getStudentsMailWithCertification(students,certification) | Obtiene array del .json y certification es un booleano, se pueden consultar los correos  que tienen certificación y de los que no |
|              3. consultar todos los estudiantes que tengan `credits` mayor a 500.              | getStudentsWithCredits(students,credits)                 | Obtiene array del .json y credits es la cantidad a comparar. Para cumplir con el requerimiento sería 500                          |

* Student Controller

| Requerimiento | Método estático                               | Parámetros                       |
| :-----------: | ----------------------------------------------- | --------------------------------- |
|       1       | getAllStudents()                                | Ninguno.                          |
|       2       | getStudentsMailWithCertification(certification) | Boolean(true or false)            |
|       3       | getStudentsWithCredits(credits)                 | Creditos es el número a comparar |

### 3. Pruebas de Unidad

Las 2 clases mencionadas anteriormente cuentan con sus respectivas pruebas de unidad las cuales se encuentran en la carpeta

[test](https://github.com/Chaylander/VisualThinking_API-CC/tree/master/test)

### 4. API

Los endpoints que contiene la API.

La API se encuentra en

[server.js](https://github.com/Chaylander/VisualThinking_API-CC/blob/master/lib/server.js)

|                     Endpoint                     | Request                                                                                               |                                              Respuesta                                              |
| :----------------------------------------------: | ----------------------------------------------------------------------------------------------------- | :-------------------------------------------------------------------------------------------------: |
|               `localhost:3000/`               | `localhost:3000/`                                                                                   |                       Mensaje de bienvenida a la API<br />de Visual Thingking                       |
|           `localhost:3000/students`           | `localhost:3000/students`                                                                           |                                   Array de todos los estudiantes                                   |
| `localhost:3000/students/certification/:state` | `localhost:3000/students/certification/true`<br />o `localhost:3000/students/certification/false` |    Regresa array de los correos de los estudiantes con(o los que no tienen)<br />certificación    |
|   `localhost:3000/students/credits/:credits`   | `localhost:3000/students/credits/500`                                                               | Regresa array de los estudiantes con más de 500 creditos (Se puede cambiar el número de creditos) |

#### Ejemplo de uso de la API

* url localhost:3000/

`{"message":"Visual Thinking Api welcome!"}`

<details>
<summary> * url `localhost:3000/students` </summary>

```
[{"id":"6264d5d89f1df827eb84bb23","name":"Warren","email":"Todd@visualpartnership.xyz","credits":508,"enrollments":["Visual Thinking Intermedio","Visual Thinking Avanzado"],"previousCourses":1,"haveCertification":true},{"id":"6264d5d85cf81c496446b67f","name":"Lucinda","email":"Sexton@visualpartnership.xyz","credits":677,"enrollments":["Visual Thinking Avanzado"],"previousCourses":6,"haveCertification":true},{"id":"6264d5d8cda17de0d2e9f236","name":"Fuentes","email":"Sharlene@visualpartnership.xyz","credits":210,"enrollments":["Visual Thinking Avanzado"],"previousCourses":10,"haveCertification":true},{"id":"6264d5d8878a117a9c57c5c4","name":"Claudia","email":"Howell@visualpartnership.xyz","credits":227,"enrollments":["Visual Thinking Avanzado"],"previousCourses":5,"haveCertification":true},{"id":"6264d5d8dd1a0be4e249c662","name":"Phillips","email":"Camacho@visualpartnership.xyz","credits":973,"enrollments":["Visual Thinking Intermedio"],"previousCourses":8,"haveCertification":false},{"id":"6264d5d8dd01ab97ddedbba5","name":"Taylor","email":"Haynes@visualpartnership.xyz","credits":652,"enrollments":["Visual Thinking Avanzado"],"previousCourses":5,"haveCertification":true},{"id":"6264d5d89d03e25451f124e5","name":"Mindy","email":"Alfreda@visualpartnership.xyz","credits":830,"enrollments":["Visual Thinking Intermedio","Visual Thinking Avanzado"],"previousCourses":9,"haveCertification":false},{"id":"6264d5d82c0b4c7dfb0b6ad5","name":"Kara","email":"Simon@visualpartnership.xyz","credits":833,"enrollments":["Visual Thinking Avanzado"],"previousCourses":8,"haveCertification":false},{"id":"6264d5d88214a862261ab1a3","name":"Young","email":"Montoya@visualpartnership.xyz","credits":447,"enrollments":["Visual Thinking Avanzado","Visual Thinking Básico"],"previousCourses":9,"haveCertification":true},{"id":"6264d5d876e13bc0a8ee0bff","name":"Constance","email":"Benton@visualpartnership.xyz","credits":311,"enrollments":["Visual Thinking Intermedio","Visual Thinking Básico"],"previousCourses":7,"haveCertification":true},{"id":"6264d5d8a77fc9d454044d98","name":"Cora","email":"Shari@visualpartnership.xyz","credits":921,"enrollments":["Visual Thinking Avanzado","Visual Thinking Intermedio"],"previousCourses":3,"haveCertification":false},{"id":"6264d5d84b49870f8ac742ed","name":"Terra","email":"Jasmine@visualpartnership.xyz","credits":388,"enrollments":["Visual Thinking Avanzado"],"previousCourses":5,"haveCertification":false},{"id":"6264d5d87e5aaed2d52b7bf6","name":"Roxanne","email":"Dionne@visualpartnership.xyz","credits":964,"enrollments":["Visual Thinking Básico","Visual Thinking Avanzado"],"previousCourses":10,"haveCertification":true},{"id":"6264d5d8ed556685cf55a58e","name":"Whitney","email":"Wells@visualpartnership.xyz","credits":186,"enrollments":["Visual Thinking Básico"],"previousCourses":8,"haveCertification":false},{"id":"6264d5d8b89c3c603f0c7db5","name":"Dina","email":"Marissa@visualpartnership.xyz","credits":341,"enrollments":["Visual Thinking Intermedio"],"previousCourses":6,"haveCertification":false},{"id":"6264d5d8b1cc4cd567bcf354","name":"Bennett","email":"Martin@visualpartnership.xyz","credits":707,"enrollments":["Visual Thinking Intermedio"],"previousCourses":3,"haveCertification":false},{"id":"6264d5d8d88c29406af17d12","name":"Burke","email":"Marie@visualpartnership.xyz","credits":397,"enrollments":["Visual Thinking Avanzado","Visual Thinking Intermedio"],"previousCourses":4,"haveCertification":false},{"id":"6264d5d8b7b5eaccfc8f51f1","name":"Bessie","email":"Mcpherson@visualpartnership.xyz","credits":558,"enrollments":["Visual Thinking Intermedio","Visual Thinking Básico"],"previousCourses":4,"haveCertification":true},{"id":"6264d5d8b4b46a11ea710c21","name":"Obrien","email":"Gracie@visualpartnership.xyz","credits":876,"enrollments":["Visual Thinking Avanzado","Visual Thinking Intermedio"],"previousCourses":3,"haveCertification":true},{"id":"6264d5d8d6646dc0187f08d3","name":"Orr","email":"Nikki@visualpartnership.xyz","credits":225,"enrollments":["Visual Thinking Básico","Visual Thinking Avanzado"],"previousCourses":10,"haveCertification":false},{"id":"6264d5d82b41e37ba48194cd","name":"Lynda","email":"Dee@visualpartnership.xyz","credits":683,"enrollments":["Visual Thinking Avanzado","Visual Thinking Básico"],"previousCourses":1,"haveCertification":false},{"id":"6264d5d819f2106c68b1e87c","name":"Carey","email":"Lakisha@visualpartnership.xyz","credits":591,"enrollments":["Visual Thinking Avanzado"],"previousCourses":3,"haveCertification":false},{"id":"6264d5d85dd337ec37a708d4","name":"Gilda","email":"Julianne@visualpartnership.xyz","credits":576,"enrollments":["Visual Thinking Básico"],"previousCourses":4,"haveCertification":false},{"id":"6264d5d80a5f5cdd35f740f5","name":"Lourdes","email":"Ila@visualpartnership.xyz","credits":371,"enrollments":["Visual Thinking Básico","Visual Thinking Intermedio"],"previousCourses":5,"haveCertification":true},{"id":"6264d5d887889da7b6832d61","name":"Elba","email":"Teresa@visualpartnership.xyz","credits":857,"enrollments":["Visual Thinking Básico","Visual Thinking Intermedio"],"previousCourses":4,"haveCertification":false},{"id":"6264d5d87a139ff2d683548c","name":"Wall","email":"Dorthy@visualpartnership.xyz","credits":919,"enrollments":["Visual Thinking Avanzado","Visual Thinking Intermedio"],"previousCourses":6,"haveCertification":true},{"id":"6264d5d8e494a32f84946435","name":"Leona","email":"Mcfarland@visualpartnership.xyz","credits":465,"enrollments":["Visual Thinking Avanzado","Visual Thinking Básico"],"previousCourses":6,"haveCertification":true},{"id":"6264d5d877237bd09c903d33","name":"Cecile","email":"Joyce@visualpartnership.xyz","credits":564,"enrollments":["Visual Thinking Intermedio","Visual Thinking Avanzado"],"previousCourses":7,"haveCertification":false},{"id":"6264d5d8f99e1eae7392b146","name":"Alvarado","email":"Maryann@visualpartnership.xyz","credits":450,"enrollments":["Visual Thinking Avanzado","Visual Thinking Intermedio"],"previousCourses":10,"haveCertification":true},{"id":"6264d5d85d181c123117e4f7","name":"Reyna","email":"Alana@visualpartnership.xyz","credits":777,"enrollments":["Visual Thinking Básico","Visual Thinking Intermedio"],"previousCourses":2,"haveCertification":true},{"id":"6264d5d8df8a628bf8b62430","name":"Richards","email":"Hines@visualpartnership.xyz","credits":552,"enrollments":["Visual Thinking Básico","Visual Thinking Avanzado"],"previousCourses":10,"haveCertification":false},{"id":"6264d5d86fdaf46cfec586c6","name":"Lindsey","email":"Rios@visualpartnership.xyz","credits":585,"enrollments":["Visual Thinking Avanzado","Visual Thinking Básico"],"previousCourses":4,"haveCertification":false},{"id":"6264d5d88775eed976fa79db","name":"Lowery","email":"Rosemary@visualpartnership.xyz","credits":436,"enrollments":["Visual Thinking Básico","Visual Thinking Avanzado"],"previousCourses":4,"haveCertification":true},{"id":"6264d5d8e24ab574c5088fb5","name":"Jenny","email":"Keith@visualpartnership.xyz","credits":156,"enrollments":["Visual Thinking Avanzado","Visual Thinking Intermedio"],"previousCourses":3,"haveCertification":true},{"id":"6264d5d8ad01712a767dcf93","name":"Margret","email":"Delaney@visualpartnership.xyz","credits":578,"enrollments":["Visual Thinking Intermedio"],"previousCourses":4,"haveCertification":true},{"id":"6264d5d8fa328a90447195dc","name":"Sherman","email":"Landry@visualpartnership.xyz","credits":244,"enrollments":["Visual Thinking Avanzado","Visual Thinking Básico"],"previousCourses":3,"haveCertification":false},{"id":"6264d5d8b3f41b0cb2df21f1","name":"Kay","email":"Ball@visualpartnership.xyz","credits":281,"enrollments":["Visual Thinking Básico","Visual Thinking Avanzado"],"previousCourses":10,"haveCertification":true},{"id":"6264d5d80ec0e36d3024224b","name":"Laverne","email":"Head@visualpartnership.xyz","credits":627,"enrollments":["Visual Thinking Intermedio","Visual Thinking Básico"],"previousCourses":10,"haveCertification":false},{"id":"6264d5d81bf954ddff848dfc","name":"Perry","email":"Sally@visualpartnership.xyz","credits":444,"enrollments":["Visual Thinking Básico","Visual Thinking Intermedio"],"previousCourses":4,"haveCertification":true},{"id":"6264d5d86a73abf2e8bcc94f","name":"Ayers","email":"Tricia@visualpartnership.xyz","credits":935,"enrollments":["Visual Thinking Avanzado","Visual Thinking Intermedio"],"previousCourses":8,"haveCertification":false},{"id":"6264d5d887b0ee0adfdf9442","name":"Franco","email":"Antoinette@visualpartnership.xyz","credits":335,"enrollments":["Visual Thinking Intermedio","Visual Thinking Básico"],"previousCourses":4,"haveCertification":true},{"id":"6264d5d864550a6a867d8e22","name":"Rosanne","email":"Juliette@visualpartnership.xyz","credits":469,"enrollments":["Visual Thinking Avanzado"],"previousCourses":3,"haveCertification":true},{"id":"6264d5d82701c8ec81325351","name":"Mann","email":"Harding@visualpartnership.xyz","credits":335,"enrollments":["Visual Thinking Avanzado","Visual Thinking Intermedio"],"previousCourses":5,"haveCertification":true},{"id":"6264d5d86119fd599c5a67e7","name":"Tillman","email":"Kerri@visualpartnership.xyz","credits":600,"enrollments":["Visual Thinking Intermedio","Visual Thinking Avanzado"],"previousCourses":5,"haveCertification":false},{"id":"6264d5d8197a2877933df120","name":"Mosley","email":"Dixon@visualpartnership.xyz","credits":529,"enrollments":["Visual Thinking Avanzado"],"previousCourses":6,"haveCertification":true},{"id":"6264d5d8cb3e0b1d210f96c6","name":"Carolina","email":"Sonia@visualpartnership.xyz","credits":487,"enrollments":["Visual Thinking Básico","Visual Thinking Avanzado"],"previousCourses":8,"haveCertification":false},{"id":"6264d5d81872b70bb0c171a6","name":"Monroe","email":"Beulah@visualpartnership.xyz","credits":329,"enrollments":["Visual Thinking Avanzado"],"previousCourses":8,"haveCertification":true},{"id":"6264d5d85659057b4331973b","name":"Chase","email":"Moses@visualpartnership.xyz","credits":769,"enrollments":["Visual Thinking Avanzado","Visual Thinking Intermedio"],"previousCourses":3,"haveCertification":true},{"id":"6264d5d8e384c8da8ea5c16b","name":"Ware","email":"Shields@visualpartnership.xyz","credits":552,"enrollments":["Visual Thinking Avanzado","Visual Thinking Intermedio"],"previousCourses":4,"haveCertification":true},{"id":"6264d5d8aa87c9330234cbc2","name":"Hollie","email":"Jewell@visualpartnership.xyz","credits":376,"enrollments":["Visual Thinking Intermedio","Visual Thinking Básico"],"previousCourses":5,"haveCertification":true},{"id":"6264d5d82eb0f4917bd0d332","name":"Clare","email":"Hays@visualpartnership.xyz","credits":227,"enrollments":["Visual Thinking Intermedio","Visual Thinking Básico"],"previousCourses":2,"haveCertification":true}]
```

</details>

<details>
<summary> * url `localhost:3000/students/certification/true` </summary>

``["Todd@visualpartnership.xyz","Sexton@visualpartnership.xyz","Sharlene@visualpartnership.xyz","Howell@visualpartnership.xyz","Haynes@visualpartnership.xyz","Montoya@visualpartnership.xyz","Benton@visualpartnership.xyz","Dionne@visualpartnership.xyz","Mcpherson@visualpartnership.xyz","Gracie@visualpartnership.xyz","Ila@visualpartnership.xyz","Dorthy@visualpartnership.xyz","Mcfarland@visualpartnership.xyz","Maryann@visualpartnership.xyz","Alana@visualpartnership.xyz","Rosemary@visualpartnership.xyz","Keith@visualpartnership.xyz","Delaney@visualpartnership.xyz","Ball@visualpartnership.xyz","Sally@visualpartnership.xyz","Antoinette@visualpartnership.xyz","Juliette@visualpartnership.xyz","Harding@visualpartnership.xyz","Dixon@visualpartnership.xyz","Beulah@visualpartnership.xyz","Moses@visualpartnership.xyz","Shields@visualpartnership.xyz","Jewell@visualpartnership.xyz","Hays@visualpartnership.xyz"]``

</details>

<details>
<summary> * url `localhost:3000/students/credits/500` </summary>

``[{"id":"6264d5d89f1df827eb84bb23","name":"Warren","email":"Todd@visualpartnership.xyz","credits":508,"enrollments":["Visual Thinking Intermedio","Visual Thinking Avanzado"],"previousCourses":1,"haveCertification":true},{"id":"6264d5d85cf81c496446b67f","name":"Lucinda","email":"Sexton@visualpartnership.xyz","credits":677,"enrollments":["Visual Thinking Avanzado"],"previousCourses":6,"haveCertification":true},{"id":"6264d5d8dd1a0be4e249c662","name":"Phillips","email":"Camacho@visualpartnership.xyz","credits":973,"enrollments":["Visual Thinking Intermedio"],"previousCourses":8,"haveCertification":false},{"id":"6264d5d8dd01ab97ddedbba5","name":"Taylor","email":"Haynes@visualpartnership.xyz","credits":652,"enrollments":["Visual Thinking Avanzado"],"previousCourses":5,"haveCertification":true},{"id":"6264d5d89d03e25451f124e5","name":"Mindy","email":"Alfreda@visualpartnership.xyz","credits":830,"enrollments":["Visual Thinking Intermedio","Visual Thinking Avanzado"],"previousCourses":9,"haveCertification":false},{"id":"6264d5d82c0b4c7dfb0b6ad5","name":"Kara","email":"Simon@visualpartnership.xyz","credits":833,"enrollments":["Visual Thinking Avanzado"],"previousCourses":8,"haveCertification":false},{"id":"6264d5d8a77fc9d454044d98","name":"Cora","email":"Shari@visualpartnership.xyz","credits":921,"enrollments":["Visual Thinking Avanzado","Visual Thinking Intermedio"],"previousCourses":3,"haveCertification":false},{"id":"6264d5d87e5aaed2d52b7bf6","name":"Roxanne","email":"Dionne@visualpartnership.xyz","credits":964,"enrollments":["Visual Thinking Básico","Visual Thinking Avanzado"],"previousCourses":10,"haveCertification":true},{"id":"6264d5d8b1cc4cd567bcf354","name":"Bennett","email":"Martin@visualpartnership.xyz","credits":707,"enrollments":["Visual Thinking Intermedio"],"previousCourses":3,"haveCertification":false},{"id":"6264d5d8b7b5eaccfc8f51f1","name":"Bessie","email":"Mcpherson@visualpartnership.xyz","credits":558,"enrollments":["Visual Thinking Intermedio","Visual Thinking Básico"],"previousCourses":4,"haveCertification":true},{"id":"6264d5d8b4b46a11ea710c21","name":"Obrien","email":"Gracie@visualpartnership.xyz","credits":876,"enrollments":["Visual Thinking Avanzado","Visual Thinking Intermedio"],"previousCourses":3,"haveCertification":true},{"id":"6264d5d82b41e37ba48194cd","name":"Lynda","email":"Dee@visualpartnership.xyz","credits":683,"enrollments":["Visual Thinking Avanzado","Visual Thinking Básico"],"previousCourses":1,"haveCertification":false},{"id":"6264d5d819f2106c68b1e87c","name":"Carey","email":"Lakisha@visualpartnership.xyz","credits":591,"enrollments":["Visual Thinking Avanzado"],"previousCourses":3,"haveCertification":false},{"id":"6264d5d85dd337ec37a708d4","name":"Gilda","email":"Julianne@visualpartnership.xyz","credits":576,"enrollments":["Visual Thinking Básico"],"previousCourses":4,"haveCertification":false},{"id":"6264d5d887889da7b6832d61","name":"Elba","email":"Teresa@visualpartnership.xyz","credits":857,"enrollments":["Visual Thinking Básico","Visual Thinking Intermedio"],"previousCourses":4,"haveCertification":false},{"id":"6264d5d87a139ff2d683548c","name":"Wall","email":"Dorthy@visualpartnership.xyz","credits":919,"enrollments":["Visual Thinking Avanzado","Visual Thinking Intermedio"],"previousCourses":6,"haveCertification":true},{"id":"6264d5d877237bd09c903d33","name":"Cecile","email":"Joyce@visualpartnership.xyz","credits":564,"enrollments":["Visual Thinking Intermedio","Visual Thinking Avanzado"],"previousCourses":7,"haveCertification":false},{"id":"6264d5d85d181c123117e4f7","name":"Reyna","email":"Alana@visualpartnership.xyz","credits":777,"enrollments":["Visual Thinking Básico","Visual Thinking Intermedio"],"previousCourses":2,"haveCertification":true},{"id":"6264d5d8df8a628bf8b62430","name":"Richards","email":"Hines@visualpartnership.xyz","credits":552,"enrollments":["Visual Thinking Básico","Visual Thinking Avanzado"],"previousCourses":10,"haveCertification":false},{"id":"6264d5d86fdaf46cfec586c6","name":"Lindsey","email":"Rios@visualpartnership.xyz","credits":585,"enrollments":["Visual Thinking Avanzado","Visual Thinking Básico"],"previousCourses":4,"haveCertification":false},{"id":"6264d5d8ad01712a767dcf93","name":"Margret","email":"Delaney@visualpartnership.xyz","credits":578,"enrollments":["Visual Thinking Intermedio"],"previousCourses":4,"haveCertification":true},{"id":"6264d5d80ec0e36d3024224b","name":"Laverne","email":"Head@visualpartnership.xyz","credits":627,"enrollments":["Visual Thinking Intermedio","Visual Thinking Básico"],"previousCourses":10,"haveCertification":false},{"id":"6264d5d86a73abf2e8bcc94f","name":"Ayers","email":"Tricia@visualpartnership.xyz","credits":935,"enrollments":["Visual Thinking Avanzado","Visual Thinking Intermedio"],"previousCourses":8,"haveCertification":false},{"id":"6264d5d86119fd599c5a67e7","name":"Tillman","email":"Kerri@visualpartnership.xyz","credits":600,"enrollments":["Visual Thinking Intermedio","Visual Thinking Avanzado"],"previousCourses":5,"haveCertification":false},{"id":"6264d5d8197a2877933df120","name":"Mosley","email":"Dixon@visualpartnership.xyz","credits":529,"enrollments":["Visual Thinking Avanzado"],"previousCourses":6,"haveCertification":true},{"id":"6264d5d85659057b4331973b","name":"Chase","email":"Moses@visualpartnership.xyz","credits":769,"enrollments":["Visual Thinking Avanzado","Visual Thinking Intermedio"],"previousCourses":3,"haveCertification":true},{"id":"6264d5d8e384c8da8ea5c16b","name":"Ware","email":"Shields@visualpartnership.xyz","credits":552,"enrollments":["Visual Thinking Avanzado","Visual Thinking Intermedio"],"previousCourses":4,"haveCertification":true}]``

</details>
