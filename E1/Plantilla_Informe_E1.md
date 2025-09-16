# Informe Entrega 1 - Bases de datos IIC2413

### Datos del Alumno
| **Apellidos**       | **Nombres**          | **Número de Alumno** |
|---------------------|----------------------|----------------------|
| Correa Ocampo | Pia Andrea    |2221318J              |

- Nombre de la empresa: Gatitospokemon

### 1. Modelo Entidad-Relación (E/R)
<!-- Inserta aquí tu diagrama ER. Usa el formato svg para evitar la perdida de calidad. Reemplaza "diagrama.svg" por la ruta a tu archivo -->

![Diagrama E/R](modeloerbddE1.drawio.png)

### 2. Entidades Débiles
<!-- Justifica CADA entidad débil identificada  -->
#### 2.1 Entidad_Plan_salud_ISAPRE
Esta fue la única identidad débil que identifiqué ya que el plan de salud no tiene sentido por si sólo si no está ligado a su isapre (cada isapre tiene un plan de salud), y al principio había hecho la relación directa de la entidad Isapre a Grupo Fonasa, pero como cada isapre tiene su plan, pensé que así quedaría más claro.

### 3. Llaves Primarias  y Compuestas
<!-- Justifica TODAS las llaves: primaria simple y primaria compuesta -->
#### 3.1 Entidad_Persona
La llave primaria de __Entidad Persona__ es  __id_persona__ porque a mi citerio para cada persona era mejor crear esta surrogate key, para manejarla mejor sobretodo en el esquema y porque vimos en clases que muchas veces el rut no era buena primary key. Pensé en que quizás si tenemos trabajadores que también pueden ser pacientes, no se nos dejaría inscribir 2 veces el mismo rut.
* Aquí me confundí un poco con el código de isapre que aparece en el archivo __Personas.xlsx__, creo que lo hice pensando en cruzar tablas, porque como para la institución previsional le puse de llave primaria id_institución, en vez de poner el código ministerial, decidí cambiarlo al id de cada institución en vez del código ministerial. (nosé si eso es legal 😭)
#### 3.2 Entidad_Paciente
La llave primaria de __Entidad_Paciente__ es __id_persona__ porque es una entidad hija de __Entidad Persona__ y mantiene la misma llave primaria que la entidad padre.
#### 3.3 Entidad_Trabajador
La llave primaria de __Entidad_Trabajador__ es __id_persona__ porque es una entidad hija de __Entidad Persona__ y mantiene la misma llave primaria que la entidad padre.
#### 3.4 Entidad_Administrativo
La llave primaria de __Entidad_Administrativo__ es __id_persona__ porque es una entidad hija de __Entidad Trabajador__ y mantiene la misma llave primaria que la entidad padre.
#### 3.5 Entidad_Staff_Médico
La llave primaria de __Entidad Staff Médico__ es __id_persona__ porque es una entidad hija de __Entidad Trabajador__ y mantiene la misma llave primaria que la entidad padre.
* A esta entidad decidí agregarle el atributo profesión que aparece como dato en el archivo __Personas.xlsx__, ya que se supone que sólo staff de profesión médico pueden generar órdenes y recetas y realizar consultas médicas. Antes lo había puesto en Trabajador, pero como suspuse que ningún andministrativo sería médico, quise ponerlo en la entidad __Entidad Staff Médico__.
#### 3.6 Entidad_Médico
La llave primaria de __Entidad Médico__ es __id_persona__ porque es la entidad hija de __Entidad Staff Médico__ y mantiene la misma llave primaria que la entidad padre.
#### 3.7 Entidad_Institución_previsional
La llave primaria de __Entidad Institución previsional__ es __id_institución__ porque decidí crear esta surrogate key para manejar mejor las instituciones. Podría haber usado el código ministerial pero decidí "irme a la segura" con la surrogte key para identificar cada institución: Isapre ó Fonasa.
#### 3.8 Entidad_Isapre
La llave primaria de __Entidad Isapre__ es __id_institución__ porque es una entidad hija de __Entidad Institución previsional__ y mantiene la misma llave primaria que la entidad padre.
#### 3.9 Entidad_Fonasa
La llave primaria de __Entidad Fonasa__ es __id_institución__ porque es una entidad hija de __Entidad Institución previsional__ y mantiene la misma llave primaria que la entidad padre.
#### 3.10 Entidad_Plan_salud_ISAPRE
La llave parcial de __Entidad Plan salud ISAPRE__ es la llave compuesta (__id_institución__, __nombre__) porque es una entidad débil, y depende de __Entidad Isapre__, entonces su llave se compone de un de sus atributos y la llave de la identidad a la que depende.
#### 3.11 Entidad_Grupo_Arancel_Fonasa
La llave primaria de __Entidad Grupo Arancel Fonasa__ es __id_grupo__ porque para identificar a cada grupo crí que sería mejor crear una surrogate key, por qu eventualmente el nombre del grupo podría cambiar ó cosas de ese estilo.
* Decidí hacer el grupo una entidad por si sola para relacionar de manera directa cada grupo con su bonificación según el plan de cada isapre, específicamente para la regla de negocio n° 7, así quedaría más directo y más claro.
#### 3.12 Entidad_Consulta_médica
La llave primaria de __Entidad Consulta médica__ es la llave __id_consulta__ (surrogate key) para identificar cada consulta y/o atención médica, ya que el nombre podría cambiar.
* Esta entidad la creé para identificar cada consulta, y le agregué atributos de diagnostico, fecha, id_medico y id_paciente. Los id los obtengo como foreign key de Paciente(id_persona) y Médico(id_persona).
#### 3.13 Entidad_Receta
La llave primaria de __Entidad Receta__ es la llave __id_receta__ (surrogate key) para identificar cada receta, sobretodo porque en los ejemplos y en el enunciado se habla de un conjunto de medicamentos y no sólo un medicamento. Le agregué el tipo de receta, para saber si  es fármacos, refrigerados, sueros ó psicotrópicos. Además del atributo código_autorización que sería sólo para las recetas de psicotrópicos. 
* La identificación del paciente y del médico, la obtengo de la consulta médica ya que esta genera una receta, y si ponía los mismos atributos iba a quedar muy redundante. 
* También pensé en cómo encontraría un medicamento en Maestro Farmacia, pero es un conjunto de medicamentos y tanto en el ejemplo como el enunciado solo se mencionan los nombres de los medicamentos, no los códigos genéricos, o alguno de los atributos de Maestro Farmacia.
#### 3.14 Entidad_Orden
La llave primaria de __Entidad Orden__ es la llave __id_orden__ (surrogate key) para identificar cada orden, porque al igual que en los ejemplos y en el enunciado se habla de un conjunto de exámenes y no sólo un examen. Le agregué también la firma_doc, no sé si sea necessaria, la puse solamente porque el enunciado lo mencionaba.
* Al igual que en __Entidad Receta__, la identificación del paciente y del médico, la obtengo de la consulta médica.
* También pensé en cómo encontraría un examen en Arancel Fonasa, pero es un conjunto de exámenes y tanto en el ejemplo como el enunciado solo se mencionan los nombres de los exámenes, no los códigos de fonasa, o alguno de los atributos de Arancel Fonasa.
#### 3.15 Entidad_Arancel_Fonasa
La llave primaria de __Entidad Arancel Fonasa__ es la llave compuesta (__código__, __código_adicional__) porque según lo que observé en el archivo __Arancelfonasa.xlsx__ el código va aumentando en uno al igual que un id, y para asegurame le agregué el código adicional.
* Acá al abrir el archivo __Arancelfonasa.xlsx__ en la columna de Código Adicional no me aparecía nada, pero como pregunté en una issue cuales eran sus columnas, me dijeron que es sí. Es decir, si tiene valores y en ninguna parte se especifica si tiene valores nulos o no.
#### 3.16 Entidad_Maestro_Farmacia
La llave primaria de __Entidad Maestro Farmacia__ es la llave __código_genérico__ porque al ver el archivo __MaestroFramacia.xlsx__, el código genérico va aumentando en 1 al igual que un id, por eso decidí que sería una buena llave primaria.
#### 3.17 Entidad_Arancel_DCColita_de_rana
La llave primaria de __Entidad Arancel DCColita de rana__ es la llave __código_fonasa__ porque en el enunciado dice que corresponde al código + código adicional, y al ser esa a llave de la __Entidad Arancel Fonasa__, decidí que sería una buena llave primaria de esta entidad.

### 4. Relaciones
<!-- Justifica TODAS las relaciones de tu modelo -->
#### 4.1 Relacion_afiliada
Relaciona la __Entidad Persona__ y la __Entidad Institución previsional__, porque una persona contrata una institución previsional sea Isapre o Fonasa, ó también puede ser particular. (se explica en la cardinalidad)
A esta relaión le agregué el atributo __tipo__ que representa si la persona es titular ó beneficiario.
* Para esto no supe cómo representar las reglas de beneficiario y titular, en un principio había hecho una entidad Titular y otra Beneficiario, y ambas se relacionaban con Institución, pero quedaba redundante y extraño. 
#### 4.2 Relacion_tiene
Relaciona la __Entidad Isapre__ y la __Entidad Plan salud ISAPRE__ (entidad débil), porque cada isapre tiene su propio plan de salud, y el el plan de salud necesita la isapre para existir.
#### 4.3 Relacion_bonificación
Relaciona la __Entidad Plan salud ISAPRE__ y la __Entidad Grupo Arancel Fonasa__, porque un plan de salud bonifica a cada grupo del arancel fonasa. Esta relación incluye el atributo __porcentaje__ porquela bonificación de la isapre consiste en el porcentaje de bonificación por cada grupo.
#### 4.4 Relacion_realiza
Relaciona la __Entidad Médico__ y la __Entidad Consulta Médica__, porque un médico realiza las consultas y/o atenciones médicas.
#### 4.5 Relacion_emite
Relaciona la __Entidad Médico__ y la __Entidad Receta__, porque el médico es quién emite recetas de medicamentos.
También relaciona la __Entidad Médico__ y la __Entidad Orden__, porque el médico es quién emite órdenes de exámenes y procedimientos.
#### 4.6 Relacion_se_genera
Relaciona la __Entidad Consulta Médica__ y la __Entidad Receta__, porque en una consulta médica se generan las recetas de medicamentos.
También relaciona la __Entidad Consulta Médica__ y la __Entidad Orden__, porque en una consulta médica se generan las órdenes de exámenes y procedimientos.
#### 4.7 Relacion_registrada_en
Relaciona la __Entidad Receta__ y la __Entidad Maestro Farmacia__, porque la recetas están registradas en Maestro Farmacia.
También relaciona la __Entidad Orden__ y la __Entidad Arancel Fonasa__, porque las órdenes y procedimientos est+an registradas en Arancel Fonasa.
#### 4.8 Relacion_se_conecta_con
Relaciona la __Entidad Arancel Fonasa__ y la __Entidad Arancel DCColita de rana__, porque según el enunciado, si el paciente no está afiliado a Fonasa, se cobr el valor particular del centro médico, y si es Ispre se aplica la bonificación. Entonces para saber el valor del centro, se busca el código_fonasa de Arancel Fonasa y se ve el precio, por esto están conectados.
#### 4.9 Relacion_es_de
Relaciona la __Entidad Arancel Fonasa__ y la __Entidad Grupo Arancel Fonasa__, porque, el grupo de Arancel Fonasa se obtiene de Arancel Fonasa, para luego hacer la bonificación según la Isapre.

### 5. Cardinalidades
<!-- Explica la cardinalidad en CADA relación del modelo -->
#### 5.1 Entidad_Persona - Entidad_Institución_previsional (0 a 1 -- n)
- Una instancia de **Entidad_Persona** puede estar asociada con cero o una instancias de **Entidad_Institución_previsional**. Osea que una persona puede o no estar afiliada a una institución previsional.
- Cada instancia de **Entidad_Institución_previsional** se relaciona con n instancias de **Entidad_Persona**. Es decir, que una institución previsional puede tener muchas personas afiliadas.
#### 5.2 Entidad_Isapre - Entidad_Plan_salud_ISAPRE (1 -- 1)
- Una instancia de **Entidad_Isapre** está asociada exactamente a una instancia de **Entidad_Plan_salud_ISAPRE**. Es decir, que cada isapre tiene sólo 1 plan de salud.
- Cada instancia de **c** se relaciona con n instancias de **Entidad_Isapre**. Es decir, que cada plan de salud pertenece sólo a 1 isapre.
#### 5.3 Entidad_Plan_salud_ISAPRE - Entidad_Grupo_Arancel_Fonasa (1 a n -- 1 a n)
- Una instancia de **Entidad_Plan_salud_ISAPRE** puede estar asociada a una o más instancias de **Entidad_Grupo_Arancel_Fonasa**. Es decir, que plan de salud tiene 1 o más grupos con porcentaje de bonificación.
- Cada instancia de **Entidad_Grupo_Arancel_Fonasa** puede estar asociada a una o más instancias de **Entidad_Plan_salud_ISAPRE**. Es decir, que cada grupo del arancel fonasa, puede tener 1 o más planes de salud par el cual bonifica la isapre.
#### 5.4 Entidad_Médico - Entidad_Consulta_Médica (n -- 1)
- Una instancia de **Entidad_Médico** puede estar asociada a n instancias de **Entidad_Consulta_Médica**. Es decir, que un médico puede realizar muchas consultas.
- Cada instancia de **Entidad_Consulta_Médica** puede estar asociada a exactamente una instancia de **Entidad_Médico**. Es decir, que una consulta sólo puede realizada por un médico.
#### 5.5 Entidad_Médico - Entidad_Receta(n -- 1)
- Una instancia de **Entidad_Médico** puede estar asociada a n instancias de **Entidad_Receta**. Es decir, que un médico puede emitir muchas recetas.
- Cada instancia de **Entidad_Receta** puede estar asociada a exactamente una instancia de **Entidad_Médico**. Es decir, que una receta sólo puede ser emitida por un médico.
#### 5.6 Entidad_Médico - Entidad_Orden(n -- 1)
- Una instancia de **Entidad_Médico** puede estar asociada a n instancias de **Entidad_Orden**. Es decir, que un médico puede emitir muchas órdenes.
- Cada instancia de **Entidad_Orden** puede estar asociada a exactamente una instancia de **Entidad_Médico**. Es decir, que una orden sólo puede ser emitida por un médico.
#### 5.7 Entidad_Consulta_Médica - Entidad_Receta(0 a n -- 1)
- Una instancia de **Entidad_Consulta_Médica** puede estar asociada de 0 a n instancias de **Entidad_Receta**. Es decir, que en una consulta médica se pueden generar 0 o más recetas.
- Cada instancia de **Entidad_Receta** puede estar asociada a exactamente una instancia de **Entidad_Consulta_Médica**. Es decir, que una receta sólo puede ser generada por 1 consulta médica.
#### 5.8 Entidad_Consulta_Médica - Entidad_Orden(0 a n -- 1)
- Una instancia de **Entidad_Consulta_Médica** puede estar asociada de 0 a n instancias de **Entidad_Orden**. Es decir, que en una consulta médica se pueden generar 0 o más órdenes.
- Cada instancia de **Entidad_Orden** puede estar asociada a exactamente una instancia de **Entidad_Consulta_Médica**. Es decir, que una orden sólo puede ser generada por 1 consulta médica.
#### 5.9 Entidad_Receta - Entidad_Maestro_Farmacia(n -- n)
- Una instancia de **Entidad_Receta** puede estar asociada a n instancias de **Entidad_Maestro_Farmacia**. Es decir, que muchas recetas pueden estar registradas en Maestro Farmacia, que a la vez tienen varios medicamentos.
- Cada instancia de **Entidad_Maestro_Farmacia** puede estar asociada a n instancias de **Entidad_Receta**. Es decir, que un medicamento de Maestro farmacia puede estar registrado muchas veces en muchas recetas.
#### 5.10 Entidad_Orden - Entidad_Arancel_Fonasa(n -- n)
- Una instancia de **Entidad_Orden** puede estar asociada a n instancias de **Entidad_Arancel_Fonasa**. Es decir, que muchas órdenes pueden estar registradas en Arancel Fonasa, que a la vez tiene varios exámenes.
- Cada instancia de **Entidad_Arancel_Fonasa** puede estar asociada a n instancias de **Entidad_Orden**. Es decir, que un examen de Arancel Fonasa puede estar registrado muchas veces en muchas órdenes.
#### 5.11 Entidad_Arancel_Fonasa - Entidad_Arancel_DCColita_de_rana(1 -- 1)
- Una instancia de **Entidad_Arancel_Fonasa** está asociada exactamente a una instancia de **Entidad_Arancel_DCColita_de_rana**. Es decir, que cada arancel de fonasa está asociado solo a un arancel del centro médico. (código fonasa -> código + código adicional)
- Cada instancia de **Entidad_Arancel_DCColita_de_rana** está asociada exactamente a una instancia de **Entidad_Arancel_Fonasa**. Es decir, que cada arancel del centro médico está asociado sólo a un arancel fonasa.
#### 5.12 Entidad_Grupo_Arancel_Fonasa - Entidad_Arancel_Fonasa(n -- 1)
- Una instancia de **Entidad_Grupo_Arancel_Fonasa** puede estar asociada a n instancias de **Entidad_Arancel_Fonasa**. Es decir, que a cada grupo pueden pern¿tenecer muchos aranceles.
- Cada instancia de **Entidad_Arancel_Fonasa** está asociada exactamente a una instancia de **Entidad_Grupo_Arancel_Fonasa**. Es decir, que cada arancel fonasa está asociado sólo a un grupo.


### 6. Jerarquías
<!-- Identifica y justifica TODAS las jerarquías -->
#### 6.1 Entidad_Persona - Entidad_Paciente- Entidad_Trabajador
Se modelo una jerarquía donde **Entidad_Persona** es la entidad padre y **Entidad_Paciente** y **Entidad_Trabajador** heredan de ella, porque se tiene e archivo __Personas.xlsx__ con todos sus datos, además una persona puede ser empleado del centro médico y a la vez paciente, por esto quise modelar que de Persona se puede ser Paciente, Trabajador, ó ambas. Para exprear el solapamiento, escribí "Paciente OVERLAPS
Staff médico", esto lo vi en el libro del curso en donde se habla de jerarquía, específicamente en la página 37 y 38.
#### 6.2 Entidad_Trabajador - Entidad_Administrativo- Entidad_Staff_médico
Se modelo una jerarquía donde **Entidad_Trabajador** es la entidad padre y **Entidad_Administrativo** y **Entidad_Staff_médico** heredan de ella, porque dentro de los trabajadores del centro, pueden ser Administrativos o Staff médico. En verdad no sé si esto sté correcto pero quería expresar que existen 2 tipo de empleados, que a la vez son personas registradas en la base de datos.
#### 6.3 Entidad_Staff_médico - Entidad_Médico
Se modelo una jerarquía donde **Entidad_Staff_médico** es la entidad padre y **Entidad_Médico** hereda de ella, porque dentro del staff médico, sólo los empleados con profesión médico, pueden realizar consultas, emitir recetas y órdenes. Lo modelé como jerarquía ya que médicos es una especialización del staff médico, que al mismo tiempo son trabajadores, y también personas registradas en la base de datos.
* Acá la entidad Médico la hice sólo porque se pidió que sólo médicos podían hacer estas cosas, de hecho en la ayudantía para la E1 se preguntó esto, y nos habían dicho que daba lo mismo la profesión, sólo bastaba con que fuera staff médico.
#### 6.4 Entidad_Institución_previsional - Entidad_Isapre- Entidad_Fonasa
Se modelo una jerarquía donde **Entidad_Institución_previsional** es la entidad padre y **Entidad_Isapre** y **Entidad_Fonasa** heredan de ella, porque dentro de las instituciones previsionales están las isapres y fonasa, entonces estas son especializaciones de las instituciones y tienen los mismos atributos, por esto decidí modelarlo como jerarquía.

### 7. Esquema Relacional
<!-- Construye el esquema relacional a partir de tu Modelo E/R -->

- **Entidad_Persona**( <u>id_persona</u>: SERIAL, nombre: VARCHAR, apellido: VARCHAR, RUN: INT, correo: VARCHAR, dirección: VARCHAR, teléfono: INT, rol: VARCHAR, id_institución: SERIAL, FOREIGN KEY(id_institución) REFERENCES InstituciónPrevisional(id_institución))  
- **Entidad_Paciente**( <u>id_persona</u>: SERIAL, nombre: VARCHAR, apellido: VARCHAR, RUN: INT, correo: VARCHAR, dirección: VARCHAR, teléfono: INT, rol: VARCHAR, id_institución: SERIAL, FOREIGN KEY(id_institución) REFERENCES InstituciónPrevisional(id_institución)) 
- **Entidad_Trabajador**( <u>id_persona</u>: SERIAL, nombre: VARCHAR, apellido: VARCHAR, RUN: INT, correo: VARCHAR, dirección: VARCHAR, teléfono: INT, rol: VARCHAR, id_institución: SERIAL, FOREIGN KEY(id_institución) REFERENCES InstituciónPrevisional(id_institución))
- **Entidad_Administrativo**( <u>id_persona</u>: SERIAL, nombre: VARCHAR, apellido: VARCHAR, RUN: INT, correo: VARCHAR, dirección: VARCHAR, teléfono: INT, rol: VARCHAR, id_institución: SERIAL, FOREIGN KEY(id_institución) REFERENCES InstituciónPrevisional(id_institución))
- **Entidad_Staff_médico**( <u>id_persona</u>: SERIAL, nombre: VARCHAR, apellido: VARCHAR, RUN: INT, correo: VARCHAR, dirección: VARCHAR, teléfono: INT, rol: VARCHAR, id_institución: SERIAL, profesión: VARCHAR, FOREIGN KEY(id_institución) REFERENCES InstituciónPrevisional(id_institución))
- **Entidad_Médico**( <u>id_persona</u>: SERIAL, nombre: VARCHAR, apellido: VARCHAR, RUN: INT, correo: VARCHAR, dirección: VARCHAR, teléfono: INT, rol: VARCHAR, id_institución: SERIAL, profesión: VARCHAR, FOREIGN KEY(id_institución) REFERENCES InstituciónPrevisional(id_institución))
- **Entidad_Institución_previsional**( <u>id_institución</u>: SERIAL, nombre: VARCHAR, tipo: VARCHAR, RUT: INT, enlace: VARCHAR, código_ministerial: INT)
- **Relación_Afiliación**( <u>Persona.id_persona</u>: SERIAL, <u>InstituciónPrevisional.id_institución</u>: SERIAL, tipo: VARCHAR)
- **Entidad_Isapre**( <u>id_institución</u>: SERIAL, nombre: VARCHAR, tipo: VARCHAR, RUT: INT, enlace: VARCHAR, código_ministerial: INT)
- **Entidad_Fonasa**( <u>id_institución</u>: SERIAL, nombre: VARCHAR, tipo: VARCHAR, RUT: INT, enlace: VARCHAR, código_ministerial: INT)
- **Entidad_Plan_Salud_Isapre**( <u>Isapre.id_institución</u>: SERIAL, <u>nombre</u>: VARCHAR)
- **Entidad_Grupo_Arancel_Fonasa**( <u>id_grupo</u>: SERIAL, nombre: VARCHAR)
- **Relación_Bonificación**( <u>PlanSaludIsapre.id_institución</u>: SERIAL, <u>PlanSaludIsapre.nombre</u>: VARCHAR, <u>GrupoArancelFonasa.id_grupo</u>: SERIAL, porcentaje: INT)
- **Entidad_Consulta_Médica**( <u>id_consulta</u>: SERIAL, diagnostico: VARCHAR, fecha: DATE, id_medico: SERIAL, id_paciente: SERIAL, FOREIGN KEY(id_medico) REFERENCES Médico(id_persona), FOREIGN KEY(id_paciente) REFERENCES Paciente(id_persona))
- **Entidad_Receta**( <u>id_receta</u>: SERIAL, tipo_receta: VARCHAR, código_autorización: SERIAL, id_consulta: SERIAL)
- **Entidad_Orden**( <u>id_orden</u>: SERIAL, firma_doctor: BOOL, id_consulta: SERIAL)
- **Entidad_Arancel_Fonasa**( <u>código</u>: SERIAL, <u>código_adicional</u>: SERIAL, consulta: VARCHAR, valor_fonasa: INT, tipo: VARCHAR)
- **Entidad_Maestro_Farmacia**( <u>código_genérico</u>: SERIAL, nombre: VARCHAR, precio: INT, descripción: VARCHAR, tipo: VARCHAR, código_onu: SERIAL, clasificación_onu: VARCHAR, clasificación_interna: VARCHAR, estado_codigo_generico: VARCHAR, canasta: INT)
- **Entidad_Arancel_DCColita_de_rana**( <u>código_fonasa</u>: SERIAL, codigo_interno: SERIAL, consulta: VARCHAR, valor: INT, FOREIGN KEY(código_fonasa) REFERENCES ArancelFonasa(código, código_adicional))


### 8. Consistencia y Normalización en BCNF
<!-- Justifica la consistencia del esquema y su cumplimiento de BCNF -->

- **Consistencia:**
- La verdad no sé muy bien qué poner aquí, pero creo que mi modelo:
* Si mantiene la fidelidad al problema.
* No hay redundancia de datos (a mi criterio).
* Elegí correctamente entidades y relaciones.
* No lo compliqué más de lo necesario.
* Elegí buenas llaves primarias.
- Lo único que creo que no sup hacer fueron la reglas de negocios son restricciones de titular/beneficiario, lo de que staff médico si es paciente sólo puede ser titular, y lo de BCNF porque me cuesta mucho.
    
- **Normalización en BCNF**:
- **Persona**( <u>id_persona</u>: SERIAL, nombre: VARCHAR, apellido: VARCHAR, RUN: INT, correo: VARCHAR, dirección: VARCHAR, teléfono: INT)
* Quité rol porque si lo tuviera, no estaría cumpliendo 1NF, y también quité id_institución para ponerlo en la tabla **Afiliación**.
- **Paciente**( <u>id_persona</u>: SERIAL)
- **Trabajador**( <u>id_persona</u>: SERIAL)
- **Administrativo**( <u>id_persona</u>: SERIAL)
- **StaffMedico**( <u>id_persona</u>: SERIAL, profesion: VARCHAR)
- **Medico**( <u>id_persona</u>: SERIAL, profesion: VARCHAR)
* Acá quité los demás atributos heredados ya que todo se puede conseguir con un join con el id_persona... la verdad me cuesta mucho entender BCNF así que perdón si está muy mal ☹️.
- **InstitucionPrevisional**( <u>id_institución</u>: SERIAL, nombre: VARCHAR, tipo: VARCHAR, RUT: INT, enlace: VARCHAR, código_ministerial: INT)
- **Afiliación**( <u>Persona.id_persona</u>: SERIAL, <u>InstituciónPrevisional.id_institución</u>: SERIAL, tipo: VARCHAR)
- **Isapre**( <u>id_institución</u>: SERIAL)
- **Fonasa**( <u>id_institución</u>: SERIAL)
- **PlanSaludIsapre**( <u>Isapre.id_institución</u>: SERIAL, <u>nombre</u>: VARCHAR)
- **GrupoArancelFonasa**( <u>id_grupo</u>: SERIAL, nombre: VARCHAR)
- **Bonificación**( <u>PlanSaludIsapre.id_institución</u>: SERIAL, <u>GrupoArancelFonasa.id_grupo</u>: SERIAL, porcentaje: INT)
* Acá saqué el nombre de PlanSaludIsapre porque encontré que quedaba redundante.
- **ConsultaMedica**( <u>id_consulta</u>: SERIAL, diagnostico: VARCHAR, fecha: DATE, id_medico: SERIAL, id_paciente: SERIAL, FOREIGN KEY(id_medico) REFERENCES Médico(id_persona), FOREIGN KEY(id_paciente) REFERENCES Paciente(id_persona))
- **Receta**( <u>id_receta</u>: SERIAL, tipo_receta: VARCHAR, código_autorización: SERIAL, id_consulta: SERIAL)
- **Orden**( <u>id_orden</u>: SERIAL, firma_doctor: BOOL, id_consulta: SERIAL)
- **Arancel_Fonasa**( <u>código</u>: SERIAL, <u>código_adicional</u>: SERIAL, consulta: VARCHAR, valor_fonasa: INT, tipo: VARCHAR)
- **MaestroFarmacia**( <u>código_genérico</u>: SERIAL, nombre: VARCHAR, precio: INT, descripción: VARCHAR, tipo: VARCHAR, código_onu: SERIAL, clasificación_onu: VARCHAR, clasificación_interna: VARCHAR, estado_codigo_generico: VARCHAR, canasta: INT)
- **ArancelDCColita**( <u>código_fonasa</u>: SERIAL, codigo_interno: SERIAL, consulta: VARCHAR, valor: INT, FOREIGN KEY(código_fonasa) REFERENCES ArancelFonasa(código, código_adicional))