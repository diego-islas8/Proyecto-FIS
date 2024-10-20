# Diseño de datos

![Diagrama de diseño de datos](./UML.png)

## Índice
* ### [Clases de usuario](#clases-de-usuario-1)
    * [Usuario](#usuario)
    * [Miembro](#miembro)
    * [Alumno](#usuario)
    * [Docente](#docente)
    * [Administrativo](#administrativo)
* ### [ServidorWeb](#servidorweb-1)
* ### [Chatbot](#chatbot-1)
* ### [SICEI](#sicei-1)
    * [Alumno](#alumno-1)
    * [Asignaturas](#asignaturas)
* ### [Chatbot Database](#chatbot-database-1)
    * [Conversación](#conversación)
    * [Respuestas](#respuestas)
    * [Prompt](#prompt)
    * [Usuarios](#usuarios)

## Clases de usuario
Este conjunto de clases hace referencia a los distintos tipos de usuarios presentes en la estructura del proyecto.

### Usuario
#### **IDSesion**
|Tipo de dato|
|---|
|Int|

*Descripción*\
El atributo IDSesion hace referencia al número de la sesión activa actual, con este ID se harán las comunicaciones entre el usuario y el servidor.

#### **preguntar()**
|Argumento|Tipo de dato|
|---|---|
|Prompt| String|

#### **iniciarSesion()**
Esta es una función void que redirecciona al usuario a la pagina de login de Microsoft.

### Miembro
Clase principal de los usuarios que estan autenticados, de la cual derivan las demas.

#### **Nombre**
|Tipo de dato|
|---|
| String |

*Descripción*\
Nombre completo de las personas autenticadas.

#### **Facultad**
| Tipo de dato |
|---|
| String |

*Descripción*\
Facultad de procedencia del usuario autenticado.

*Ejemplos*
| Valor |
|---|
| Facultad de matemáticas |
| Facultad de medicina |
| Facultad de derecho |

#### **FechaDeInicio**
| Tipo de dato |
|---|
| Int |

*Descripción*

Fecha de inicio de las labores o curso escolar dentro de la institución en formato [Unix timestamp](https://en.wikipedia.org/wiki/Unix_time).

*Ejemplos*
| Valor | Representación |
| --- | --- |
| 1723593600 | martes, 13 de agosto de 2024 18:00:00 GMT-06:00 |

#### **Conversaciones**
| Tipo de dato |
| --- |
| Int[] |

*Descripción*

Una lista de los ID de las conversaciones que previamente se ha tenido con el ChatBot, con tal ID se puede obtener la información completa de las conversaciones

*Ejemplos*
| Valor | Representación |
|---|---|
| [2, 4, 6] | Hace referencia que las conversaciones con las ID 2, 4 y 6 han sido llevadas por este usuario |

#### **Token**
| Tipo de dato |
| --- |
| String |

*Descripción*

Un token guardada en una cookie generada luego de realizar el proceso de [OAuth de Microsoft](https://learn.microsoft.com/es-es/entra/architecture/auth-oauth2), se utiliza para comprobar la identidad del usuario y apartir de ello acceder a información protegida.

*Ejemplo*
| Valor |
|---|
| abcde12345 |

#### **Correo**
| Tipo de dato |
|---|
| String |

*Descripción*

Correo del usuario

*Ejemplo*
| Valor |
|---|
| A20200000@alumnos.uady.mx |

### Alumno
#### **SemestreEnCurso**
| Tipo de dato |
| --- |
| Int |

*Descripción*

El número del semestre que actualmente cursa el alumno.

*Ejemplos*
| Valor | Representación |
| --- | --- |
| 6 | 6to semestre |

#### **Matricula**
| Tipo de dato |
| --- |
| String |

*Descripción*

La matricula del alumno.

*Ejemplo*
| Valor |
|---|
| A20200000 |

#### **Créditos**
| Tipo de dato |
| --- |
| Int |

*Descripción*

El total de créditos que el alumno ha alcanzado hasta el momento.

*Ejemplo*

| Valor |
| 15 |

#### **obtenerCalificaciones()**
*Descripción*

Función que permite obtener al alumno sus calificaciones de las materias a las que se ha inscrito en el semestre en curso.

#### **obtenerHorario()**
*Descripción*

Función que permite al alumno obtener las horas, salones, y maestros de las materias a las  que se ha inscrito en el semestre en curso.

#### **obtenerAsignaturas()**
*Descripción*

Función que regresa las asignaturas que se cursan en el semestre en curso.

### Docente
#### **Materias**
| Tipo de dato |
| --- |
| String[] |

*Descripción*

Una lista de las materias que el docente imparte.

*Ejemplo*
| Valor |
| --- |
| [Calculo diferencial, Calculo Integral] |

#### **Especialización**
| Tipo de dato |
| --- |
| String |

*Descripción*

Especialización del docente.

#### **NumContrato**
| Tipo de dato |
| --- |
| Int |

*Descripción*

El número de contrato del docente.

### Administrativo
#### **Departamento**
| Tipo de dato |
|---|
| String |

*Descripción*

Departamento al que pertenece el personal administrativo.

#### **Puesto**
| Tipo de dato |
| --- |
| String |

*Descripción*

Puesto administrativo del usuario.

#### **NumContrato**
| Tipo de dato |
| --- |
| Int |

*Descripción*

El número de contrato del personal administrativo.

## ServidorWeb
Clase que interpreta a la entidad del servidor web, la cual maneja las peticiones HTTP.

### IP
| Tipo de dato |
|---|
| String |

*Descripción*

La IP que va a usar el servidor.

### Port
| Tipo de dato |
| --- |
| Int |

*Descripción*

El puerto en el que el servidor web va a funcionar.

### Dominio
| Tipo de dato |
| --- |
| String |

*Descripción*

Dominio a utilizar para el servidor.

### get()
*Descripción*

Función que maneja las peticiones `GET` del protocolo HTTP.

### validarToken()
*Descripción*

Función que valida el token del OAuth de Microsoft, si determina que es correcto entonces envía una cookie con el token de autenticación para el login del usuario.

### post()
*Descripción*

Función que maneja las peticiones `POST` del protocolo HTTP, funcionará principalmente para recibir los prompts del chatbot.

## Chatbot
Clase que maneja todo lo relacionado con la lógica del chatbot.

### Status
| Tipo de dato |
|---|
| Int |

*Descripción*

La disponibilidad del chatbot en el instante.

*Ejemplos*
| Valor | Representación |
| --- | --- |
| 0 | Ok |
| 1 | Sobresaturado |
| 2 | En mantenimiento |


### Lenguajes
| Tipo de dato |
| --- |
| String[] |

*Descripción*

Array que contiene los lenguajes admitidos por el chatbot.

*Ejemplo*
| Valor |
| --- |
| [ Español, Inglés, Maya ] |

### conexionesActivas
| Tipo de dato |
| --- |
| Int[] |

*Descripción*

Lista que guarda todas las [IDSesion](#idsesion) activas en este momento.

### procesarLenguaje()
*Descripción*

Recibe el prompt del usuario y lo analiza, determina en que lenguaje está escrito y qué es lo que requiere.

### obtenerContexto()
*Descripción*

Las conversaciones anteriores que se han tenido con el bot se analizan para obtener el contexto de las partes más importantes y con ello generar una respuesta más precisa.

### consultarSICEI()
*Descripción*

Función que se encarga de hacer las peticiones a la base datos del [SICEI](#sicei-1).

### generarRespuesta()
*Descripción*

Con los datos analizados en las funciones [procesarLenguaje()](#procesarlenguaje) y [obtenerContexto()](#obtenercontexto) genera una respuesta que se envía de vuelta al [servidor web](#servidorweb) y posterior a ello se envia la respuesta al usuario.

### ponerEnCola()
*Descripción*

Si la petición no puede ser procesada en este momento manda la orden de poner en cola al usuario.

## SICEI
Base de datos de la plataforma SICEI

### Alumno
Entidad alumno de la base de datos.

#### **Nombre**
| Tipo de dato |
|---|
| String |

*Descripción*

Nombre del alumno.

#### **Semestre**
| Tipo de dato |
| --- |
| Int |

*Descripción*

Semestre que el alumno cursa actualmente.

#### **Matricula**
| Tipo de dato |
| --- |
| String |

*Descripción*

Matricula del alumno

#### **AsignaturasCursadas**
| Tipo de dato |
| --- |
| Int[] |

*Descripción*

Lista de los [ID](#asignaturaID) de las asignaturas cursadas.

#### **AsignaturasCursando**
| Tipo de dato |
| --- |
| Int[] |

*Descripción*

Lista de los [ID](#asignaturaID) de las asignaturas que se estan cursando en el semenstre actual.

### Asignaturas
Tabla de la base de datos que representan las asignaturas.

#### <span id="asignaturaID">**ID**</span>
| Tipo de dato |
| --- |
| Int |

*Descripción*

Número que identifica a la asignatura, funcionando como su identificador.

#### **Nombre**
| Tipo de dato |
| --- |
| String |

*Descripción*

El nombre de la asignatura.

#### **Creditos**
| Tipo de dato |
| --- |
| Int |

*Descripción*

Cantidad de créditos que vale la asignatura.

#### **Semestre**
| Tipo de dato |
| --- |
| Int |

*Descripción*

Semestre a la que la asignatura pertenece.

#### **TipoFormación**
| Tipo de dato |
| --- |
| String |

*Descripción*

El nivel del tipo de formación de la materia, entre los valores `Institucional`, `Disciplinar`, `Especialidad`.

#### **Secuencial**
| Tipo de dato |
| --- |
| Bool |

*Descripción*

Determina si la asignatura es secuencial o no.

#### **AsignaturaPrevia**
| Tipo de dato |
| --- |
| Int |

*Descripción*

Si la asignatura es [secuencial](#secuencial) y tiene una asignatura previa su ID será guardada en esta propiedad.

#### **AsignaturaPosterior**
| Tipo de dato |
| --- |
| Int |

*Descripción*

Si la asignatura es [secuencial](#secuencial) y tiene una asignatura posterior su ID será guardada en esta propiedad.

## ChatBot Database
Conjunto de entidades que representan los difernetes tipos de tablas a guardar en la base de datos del chatbot.

### Conversación
#### **ID**
| Tipo de dato |
| --- |
| Int |

*Descripción*

Identificador de la conversación.

#### **Prompts**
| Tipo de dato |
| --- |
| Int[] |

*Descripción*

Lista de [ID](#IDPrompt) de los prompts que va haciendo el usuario en en esa conversación

#### **Historial**
| Tipo de dato |
| --- |
| Int[] |

*Descripción*

Lista de [ID](#IDRespuestas) de las respuestas que el chatBot va generando.

#### **UltimaInteraccion**
| Tipo de dato |
| --- |
| Int |

*Descripción*

Tiempo de la ultima interacción que hubo en esta conversación en [Unix Timestamp](https://en.wikipedia.org/wiki/Unix_time)

### Respuestas
#### <label id="IDRespuestas">**ID**</label>
| Tipo de dato |
| --- |
| Int |

*Descripción*

Identificador de la respuesta.

#### **Content**
| Tipo de dato |
| --- |
| String |

*Descripción*

La cadena de texto generada por el chatbot.

#### **Rating**
| Tipo de dato |
| --- |
| Int |

*Descripción*

La calificación que el usuario le ha dado a la respuesta.

#### **Lenguaje**
| Tipo de dato |
| --- |
| String |

*Descripción*

El lenguaje de la respuesta que se generó

### Prompt
#### <label id="IDPrompt">**ID**</label>
| Tipo de dato |
| --- |
| Int |

*Descripción*

Identificador del prompt.

#### Content
| Tipo de dato |
| --- |
| String |

*Descripción*

Contenido del prompt que el usuario haya perguntado al chatbot

#### Lenguaje
| Tipo de dato |
|---|
| String |

*Descripción*
El lenguaje empleado en el contenido del prompt

#### Invalid
| Tipo de dato |
| --- |
| Bool |

*Descripción*
Determina si el prompt solicitó una información que no se le puede proporcionar.

### Usuarios
#### **Nombre**
| Tipo de dato |
| --- |
| String |

*Descripción*

Nombre del usuario almacenado en la base de datos.

#### **TokensActivos**
| Tipo de dato |
| --- |
| String[] |

*Descripción*

Lista de los tokens generados para el inicio de sesión del usuario en cuestión

#### **Conversaciones**
| Tipo de dato |
| --- |
| Int[] |

*Descripción*

Lista de las [ID](#id-1) de las conversaciones que el usuario ha llevado a cabo.