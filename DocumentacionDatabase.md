## DOCUMENTACION BASE DE DATOS

### ***\> Entidades Principales*** 
1.	*Medicamento*: Representa los productos vendidos, con atributos como clave, nombre, fecha de caducidad, descripción, precio de venta y cantidad. Esto garantiza la identificación única y el control de cada medicamento.  
2.	*Laboratorio*: Representa los proveedores, registrando nombre, dirección, teléfono y correo electrónico. Vinculado a los medicamentos que fabrica o provee. 
3.	*Venta*: Representa las operaciones comerciales, con atributos como folio, fecha y relación con el empleado que realizó la venta. 
4.	*Empleado*: Representa al personal de la farmacia, registrando nombre y apellido, lo que permite rastrear quién realizó una venta específica. 
5.	*SustanciaActiva*: Catalogación de los ingredientes activos de los medicamentos para una búsqueda más eficiente. 
6.	*Dirección*: Modelada por separado para reutilizar información y normalizar los datos. 

### ***\> Relaciones Principales*** 
•	Sustancia_Medicamento: Relaciona medicamentos con sus ingredientes activos.   
•	Laboratorio tiene Medicamento: Vincula laboratorios con los medicamentos que fabrican o proveen.   
•	Venta incluye Medicamento: Desglosa los medicamentos vendidos en una transacción, incluyendo cantidad y subtotal.   
•	Empleado hace Venta: Registra qué empleado realizó cada venta.  

![Imagen desde Google Drive](https://drive.google.com/uc?export=view&id=1PdzK74v1TNS3sdNmOL-bcqkMPNF7IYnt)  

### ***\> Justificación del Modelo Relacional en 3FN***
El modelo relacional presentado sigue los principios de normalización hasta la Tercera Forma Normal (3FN), asegurando la eliminación de redundancia y la consistencia de los datos. 
1.	Eliminación de Redundancia y Consistencia: 
Cada tabla representa una entidad única, evitando duplicación de datos.  
> Ejemplo: La tabla SustanciaUso almacena información única sobre las sustancias activas con un identificador clave.  
2.	Descomposición para Relaciones Muchos a Muchos: 
Relaciones complejas se descomponen en tablas intermedias para mantener integridad referencial. 
> Ejemplo: Medicamento_Laboratorio y Medicamento_Ventas manejan relaciones muchos a muchos entre medicamentos, laboratorios y ventas.   
3.	Dependencias Funcionales Complejas: 
Cada atributo depende únicamente de la clave primaria.   
> Ejemplo: En Medicamento, atributos como nombre y descripcion dependen de idMedicamento. 
4.	Jerarquía Geográfica Normalizada: 
Información geográfica descompuesta en tablas específicas, evitando duplicación.  
> Ejemplo: Tablas Pais, Estado, Ciudad, Colonia, cada una con claves primarias únicas. 
5.	Modularidad y Escalabilidad: 
El diseño permite agregar nuevas entidades o relaciones sin afectar la estructura existente. 
> Ejemplo: Se pueden añadir más detalles sobre Empleado o nuevos tipos de sustancias. 
6.	Cumplimiento de la 3FN: 
Elimina dependencias parciales y transitivas, asegurando integridad referencial. 
> Ejemplo: No existen atributos que dependan indirectamente de la clave primaria. 
7.	Relación Lógica y Clara entre las Entidades: 
Cada tabla y relación refleja procesos reales del negocio. 
> Ejemplo: Empleado se relaciona con Venta, mostrando quién realizó la transacción. 
8.	Diseño Enfocado en Eficiencia y Consulta: 
Facilita la realización de consultas complejas y eficientes. 
> Ejemplo: Búsqueda de medicamentos por sustancias activas y rastreo de ventas por empleado o fecha. 


