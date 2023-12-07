# FILTRO RESPUESTAS
![drawSQL-base-de-datos-tbm-export-2023-12-07](https://github.com/Dianaalejandra1446/FILTRO/assets/139186201/08eb423a-6e1c-490b-bfe9-637771e66d42)
![FILTRO](https://github.com/Dianaalejandra1446/FILTRO/assets/139186201/de6268c7-64bc-4ed6-83f6-82e4af973f39)

## CREACION DE TABLAS
~~~
CREATE TABLE Rutas(  
    id_ruta INT AUTO_INCREMENT NOT NULL PRIMARY KEY,
    nombre_zona VARCHAR(30) NOT NULL,
    tiempo_ruta TIME NOT NULL,
    id_zona INT
);
CREATE TABLE Zonas (
    id_zona INT AUTO_INCREMENT PRIMARY KEY,
    nombre_zona VARCHAR(30) NOT NULL
);

CREATE TABLE estacionesXruta(
    id_ruta INT,
    id_parada INT
);
CREATE TABLE Estaciones(
    id_parada INT NOT NULL AUTO_INCREMENT PRIMARY KEY,
    nombre_estacion VARCHAR(50)
);
CREATE TABLE rutaXconductor (
    id_ruta INT,
    id_conductor INT,
    dia VARCHAR(50)
);
CREATE TABLE Conductores (
    id_conductor INT NOT NULL AUTO_INCREMENT PRIMARY KEY,
    nombre_conductor VARCHAR(100) NOT NULL,
    apellido_conductor VARCHAR(100) NOT NULL
);
CREATE TABLE conductorXbuses(
    id_conductor INT,
    id_bus INT
);
CREATE TABLE Buses(
    id_bus INT NOT NULL AUTO_INCREMENT PRIMARY KEY,
    num_placa INT
);

ALTER TABLE Rutas
ADD FOREIGN KEY (id_zona) REFERENCES zonas (id_zona);

ALTER TABLE estacionesxruta
ADD FOREIGN KEY (id_parada) REFERENCES estaciones (id_parada),
ADD FOREIGN KEY (id_ruta) REFERENCES rutas (id_ruta);

ALTER TABLE rutaxconductor
ADD FOREIGN KEY (id_ruta) REFERENCES rutas (id_ruta),
ADD FOREIGN KEY (id_conductor) REFERENCES conductores (id_conductor);

ALTER TABLE conductorxbuses
ADD FOREIGN KEY (id_conductor) REFERENCES conductores(id_conductor),
ADD FOREIGN KEY (id_bus) REFERENCES buses(id_bus);

ALTER TABLE rutas
CHANGE nombre_zona nombre_ruta VARCHAR(30) NOT NULL;
~~~
### INSERCION DE DATOS 
~~~
INSERT INTO zonas(nombre_zona) VALUES
('Norte'),
('Sur'),
('Oriente'),
('Occidente'),
('Floridablanca'),
('Girón'),
('Piedecuesta');

INSERT INTO rutas(nombre_ruta,tiempo_ruta,id_zona) VALUES
('Universidades','2:00:00',1),
('Café Madrid',	'2:15:00',1),
('Cacique',	'1:45:00',NULL),
('Diamantes',	'1:50:00',4),
('Terminal',	'2:00:00',4),
('Prado',	'1:30:00',NULL),
('Cabecera',	'1:30:00',NULL),
('Ciudadela',	'2:00:00',NULL),
('Punta Estrella',	'2:30:00',NULL),
('Niza',	'2:45:00',5),
('Autopista',	'2:15:00',5),
('Lagos',	'2:15:00',5),
('Centro Florida','2:30:00',NULL);

INSERT INTO estaciones(nombre_estacion) VALUES
('Colseguros'),
('Clínica Chicamocha'),
('Plaza Guarín'),
('Mega Mall'),
('UIS'),
('UDI'),
('Santo Tomás'),
('Boulevard Santander'),
('Búcaros'),
('Rosita'),
('Puerta del Sol'),
('Cacique'),
('Plaza Satélite'),
('La Sirena'),
('Provenza '),
('Fontana'),
('Gibraltar'), 
('Terminal'),
('Mutis'),
('Plaza Real');

INSERT INTO estacionesxruta(id_ruta,id_parada) VALUES
(1,	1),
(1,	2),
(1,	3),
(1,	4),
(1,	5),
(1,	6),
(1,	7),
(3,	8),
(3,	9),
(3,	10),
(3,	11),
(3,	12),
(4,	13),
(4,	14),
(4,	15),
(5,	16),
(5,	17),
(8,	18),
(8,	19),
(8,	20);

INSERT INTO conductores(nombre_conductor,apellido_conductor) VALUES
('Andrés Manuel', 'López Obrador'),
('Nicolás', 'Maduro Moros'),
('Alberto ','Fernández'),
('Luiz Inácio', 'Lula da Silva'),
('Gabriel', 'Boric'),
('Miguel', 'Díaz-Canel'),
('Daniel', 'Ortega'),
('Gustavo', 'Petro Urrego'),
('Luis', 'Arce'),
('Xiomara', 'Castro');

ALTER TABLE rutaxconductor
DROP COLUMN dia;
INSERT INTO rutaxconductor(dia,id_ruta,id_conductor) VALUES
('Lunes',	1,	5),
('Martes',	1,	5),
('Miércoles',1,	5),
('Jueves',	1,	5),
('Viernes',	2,	5),
('Sábado',	2,	5),
('Domingo',	2,	5),
('Lunes',	4,	3),
('Martes',	4,	3),
('Miércoles',   4,3),
('Jueves',	5,	3),
('Viernes',	5,	3),
('Sábado',	5,	3),
('Domingo',	5,	3),
('Lunes',	10,	10),
('Martes',	10,	10),
('Miércoles',10,10),
('Jueves',	10,	10),
('Viernes',	10,	10),
('Sábado',	11,	10),
('Domingo',	11,	10),
('Lunes',	11,	7),
('Martes',	11,	7),
('Miércoles',12,6),
('Jueves',	12,	6),
('Viernes',	12,	6),
('Sábado',	12,	6),
('Domingo',	12,	6),
(NULL,NULL,1),
(NULL,NULL,2),
(NULL,NULL,4),
(NULL,NULL,8),
(NULL,NULL,9);

ALTER TABLE conductorxbuses
ADD COLUMN dia VARCHAR(50);

ALTER TABLE conductorxbuses
drop COLUMN dia;
INSERT INTO buses(num_placa) VALUES
('XVH345'),
('XDL965'),
('XFG847'),
('XRJ452'),
('XDF459'),
('XET554'),
('XKL688'),
('XXL757');

INSERT INTO conductorxbuses(id_bus,id_conductor) VALUES
(1,5),
(3,5),
(5,5),
(6,3),
(1,3),
(3,3),
(3,10),
(5,10),
(4,10),
(7,10),
(7,7),
(7,7),
(6,6);
~~~

#### CONSULTAS
1. Cantidad de Paradas por Ruta
~~~
SELECT COUNT(ER.id_parada) AS total_paradas,R.nombre_ruta
FROM  estacionesxruta ER,rutas R
INNER JOIN estaciones E ON E.id_parada = ER.id_parada
INNER JOIN R.id_ruta = ER.id_ruta
GROUP BY ER.id_parada, ;
~~~

2. Nombre de las Paradas de la Ruta Universidades
~~~

~~~
3. Nombres de las Rutas No Programadas
~~~
SELECT E.nombre_estacion
FROM estaciones E
INNER JOIN estacionesxruta ER ON ER.id_parada = E.id_parada
INNER JOIN rutas R ON R.id_ruta = ER.id_ruta
WHERE ER.id_ruta=1;
~~~
4. Rutas Programadas sin Conductor Asignado
~~~
SELECT C.nombre_conductor,C.apellido_conductor AS Ruta_sin_Conductor
FROM conductores C
INNER JOIN rutaxconductor RC ON RC.id_conductor = C.id_conductor 
INNER JOIN rutas R ON R.id_ruta = RC.id_ruta 
WHERE RC.id_ruta IS NULL or RC.id_ruta = ' ';
~~~
5. Conductores No Asignados a la Programación
~~~

~~~
6. Buses No asignados a la Programación
~~~

~~~
7. Zonas NO Programadas
~~~
SELECT Z.nombre_zona
FROM zonas Z
INNER JOIN rutas R ON R.id_zona = Z.id_zona
WHERE Z.id_zona = 0 or Z.id_zona = ' ';
~~~
8. Programación asignada a cada conductor (Conductor, Ruta y Día)
~~~

~~~
9. Programación asignada a conductores que hacen rutas de más de dos horas
~~~

~~~
10. Nombres de Zonas y cantidad de rutas que tienen programadas (Contar)
~~~

~~~
