**ERD**
![image](/images/diseño.png)
**DDL**
**CREACION DE TABLAS **

CREATE TABLE producto ( 
	id_producto          VARCHAR(15)   NOT NULL,
	nombre               VARCHAR(70)   NOT NULL,
	id_gama              VARCHAR(50)   NOT NULL,
	cantidad_stock       SMALLINT   NOT NULL,
	precio_venta         DECIMAL(15,2)   NOT NULL,
	descripcion          TEXT   ,
	CONSTRAINT pk_producto PRIMARY KEY ( id_producto )
 );

ALTER TABLE producto ADD CONSTRAINT fk_producto_gama_producto FOREIGN KEY ( id_gama ) REFERENCES gama_producto( id_gama )

CREATE TABLE gama_producto ( 
	id_gama              VARCHAR(15)   NOT NULL,
	nombre               VARCHAR(70)   NOT NULL,
	descripcion_texto    TEXT   ,
	id_imagen            VARCHAR(15)   ,
	id_html              VARCHAR(15)   ,
	CONSTRAINT pk_gama_producto PRIMARY KEY ( id_gama )
 );

CREATE TABLE detalle_pedido ( 
	id_pedido            INTEGER   NOT NULL,
	id_producto          VARCHAR(15)   NOT NULL,
	cantidad             INTEGER   NOT NULL,
	precio_unidad        DECIMAL(15,2)   NOT NULL,
	numero_linea         SMALLINT   NOT NULL
 );


ALTER TABLE detalle_pedido ADD CONSTRAINT fk_detalle_pedido_producto FOREIGN KEY ( id_producto ) REFERENCES producto( id_producto ) ON DELETE NO ACTION ON UPDATE NO ACTION;

ALTER TABLE detalle_pedido ADD CONSTRAINT fk_detalle_pedido_pedido FOREIGN KEY ( id_pedido ) REFERENCES pedido ( id_pedido )


CREATE TABLE proveedor ( 
	id_proveedor         VARCHAR(15)   NOT NULL,
	nombres              VARCHAR(70)   NOT NULL,
	email                VARCHAR(50)   NOT NULL,
	id_telefono          VARCHAR(10)   ,
	id_direccion         VARCHAR(10)   ,
	CONSTRAINT pk_proveedor PRIMARY KEY ( id_proveedor )
 );


CREATE TABLE producto_proveedor ( 
	id_producto          VARCHAR(15)   NOT NULL,
	id_proveedor         VARCHAR(15)   NOT NULL,
	precio_suministro    DECIMAL(10,3)   NOT NULL,
	fecha_inicio_asociacion DATE   
 );

ALTER TABLE producto_proveedor ADD CONSTRAINT fk_producto_proveedor FOREIGN KEY ( id_proveedor ) REFERENCES proveedor( id_proveedor );

ALTER TABLE producto_proveedor ADD CONSTRAINT fk_producto_proveedor_producto FOREIGN KEY ( id_producto ) REFERENCES producto( id_producto );


CREATE  TABLE pedido ( 
	id_pedido            INT    NOT NULL   PRIMARY KEY,
	id_cliente           INT    NOT NULL   ,
	fecha_pedido         DATE    NOT NULL   ,
	fecha_esperada       DATE    NOT NULL   ,
	fecha_entrega        DATE       ,
	id_estado            INT    NOT NULL   ,
	comentarios          TEXT       
 );

ALTER TABLE pedido ADD CONSTRAINT fk_pedido_cliente FOREIGN KEY ( id_cliente ) REFERENCES cliente( id_cliente );

ALTER TABLE pedido ADD CONSTRAINT fk_pedido_estado_pedido FOREIGN KEY ( id_estado ) REFERENCES estado_pedido( id_estado );

CREATE  TABLE estado_pedido ( 
	id_estado            INT    NOT NULL   PRIMARY KEY,
	nombre               VARCHAR(25)    NOT NULL   
 ) 

CREATE TABLE pago ( 
	id_pago              VARCHAR(50)   NOT NULL,
	id_cliente           INTEGER   NOT NULL,
	fecha_pago           DATE   NOT NULL,
	total                DECIMAL(15,2)   NOT NULL,
	CONSTRAINT pk_pago PRIMARY KEY ( id_pago )
 );

ALTER TABLE pago ADD CONSTRAINT fk_pago_cliente FOREIGN KEY ( id_cliente ) REFERENCES cliente( id_cliente )


CREATE  TABLE cliente ( 
	id_cliente           INT    NOT NULL   PRIMARY KEY,
	nombres              VARCHAR(50)    NOT NULL   ,
	apellidos            VARCHAR(50)    NOT NULL   ,
	fax                  VARCHAR(15)    NOT NULL   ,
	id_telefono          VARCHAR(20)    NOT NULL   ,
	id_direccion         VARCHAR(100)    NOT NULL   ,
	id_pais              VARCHAR(15)    NOT NULL   ,
	id_ciudad            VARCHAR(15)    NOT NULL   ,
	id_contacto          VARCHAR(30)       ,
	codigo_postal        VARCHAR(15)       ,
	limite_credito       DECIMAL(15,2)       ,
	id_empleado_ventas   INT       
 );

ALTER TABLE cliente ADD CONSTRAINT fk_cliente_ciudad FOREIGN KEY ( id_ciudad ) REFERENCES ciudad ( id_ciudad ) ON DELETE NO ACTION ON UPDATE NO ACTION;

ALTER TABLE cliente ADD CONSTRAINT fk_cliente_direccion FOREIGN KEY ( id_direccion ) REFERENCES direccion ( id_direccion );

ALTER TABLE cliente ADD CONSTRAINT fk_cliente_pais FOREIGN KEY ( id_pais ) REFERENCES pais (id_pais );


CREATE TABLE contacto ( 
	id_contacto          INTEGER   NOT NULL,
	nombres              VARCHAR(70)   NOT NULL,
	email                VARCHAR(50)   NOT NULL,
	CONSTRAINT pk_contacto PRIMARY KEY ( id_contacto )
 );


CREATE TABLE region ( 
	id_region            VARCHAR(15)   NOT NULL,
	id_pais              VARCHAR(15)   NOT NULL,
	nombre               VARCHAR(30)   ,
	CONSTRAINT pk_region PRIMARY KEY ( id_region )
 );

ALTER TABLE region ADD CONSTRAINT fk_region_payment FOREIGN KEY ( id_pais ) REFERENCES pais (id_pais);


CREATE TABLE ciudad ( 
	id_ciudad            VARCHAR(15)   NOT NULL,
	id_region            VARCHAR(15)   NOT NULL,
	nombre               VARCHAR(30)   NOT NULL,
	CONSTRAINT unq_ciudad_id_ciudad UNIQUE ( id_ciudad ) ,
	CONSTRAINT pk_ciudad PRIMARY KEY ( id_ciudad )
 );

ALTER TABLE ciudad ADD CONSTRAINT fk_ciudad_region FOREIGN KEY ( id_region ) REFERENCES region (id_region );


CREATE TABLE pais ( 
	id_pais              VARCHAR(15)   NOT NULL,
	nombre               VARCHAR(30)   NOT NULL,
	CONSTRAINT pk_pais PRIMARY KEY ( id_pais )
 );



CREATE TABLE telefono ( 
	id_telefono          VARCHAR(10)   NOT NULL,
	numero_telefono      VARCHAR(20)   NOT NULL,
	tipo_telefono        VARCHAR(15)   ,
	CONSTRAINT pk_telefono PRIMARY KEY ( id_telefono )
 );



CREATE TABLE cliente_contacto ( 
	id_cliente           INTEGER   NOT NULL,
	id_contacto          INTEGER   NOT NULL
 );

ALTER TABLE cliente_contacto ADD CONSTRAINT fk_cliente_contacto_cliente FOREIGN KEY (id_cliente) REFERENCES cliente( id_cliente );

ALTER TABLE cliente_contacto ADD CONSTRAINT fk_cliente_contacto_contacto FOREIGN KEY (id_contacto) REFERENCES contacto( id_contacto );



CREATE TABLE oficina ( 
	id_oficina           VARCHAR(10)   NOT NULL,
	id_ciudad            VARCHAR(15)   NOT NULL,
	id_pais              VARCHAR(15)   NOT NULL,
	id_region            VARCHAR(15)   ,
	codigo_postal        VARCHAR(10)   NOT NULL,
	id_telefono          VARCHAR(10)   NOT NULL,
	id_direccion         VARCHAR(15)   NOT NULL,
	CONSTRAINT pk_oficina PRIMARY KEY ( id_oficina )
 );

ALTER TABLE oficina ADD CONSTRAINT fk_oficina_ciudad FOREIGN KEY ( id_ciudad ) REFERENCES ciudad (id_ciudad );

ALTER TABLE oficina ADD CONSTRAINT fk_oficina_pais FOREIGN KEY ( id_pais ) REFERENCES pais ( id_pais );

ALTER TABLE oficina ADD CONSTRAINT fk_oficina_direccion FOREIGN KEY ( id_direccion ) REFERENCES direccion ( id_direccion );

ALTER TABLE oficina ADD CONSTRAINT fk_oficina_telefono FOREIGN KEY ( id_telefono ) REFERENCES telefono( id_telefono )


CREATE TABLE direccion ( 
	id_direccion         VARCHAR(15)   NOT NULL,
	tipo_via1            VARCHAR(10)   NOT NULL,
	numero_via1          INTEGER   NOT NULL,
	tipo_via2            VARCHAR(10)   NOT NULL,
	numero_via2          INTEGER   NOT NULL,
	barrio               VARCHAR(20)   ,
	CONSTRAINT pk_direccion PRIMARY KEY ( id_direccion )
 ); 


CREATE TABLE empleado ( 
	id_empleado          INTEGER   NOT NULL,
	nombres              VARCHAR(50)   NOT NULL,
	apellidos            VARCHAR(50)   NOT NULL,
	email                VARCHAR(100)   NOT NULL,
	id_oficina           VARCHAR(10)   NOT NULL,
	id_jefe              INTEGER   NOT NULL,
	puesto               VARCHAR(50)   ,
	CONSTRAINT pk_empleado PRIMARY KEY ( id_empleado )
 );

ALTER TABLE empleado ADD CONSTRAINT fk_empleado_oficina FOREIGN KEY ( id_oficina ) REFERENCES oficina( id_oficina );

ALTER TABLE empleado ADD CONSTRAINT fk_empleado_empleado FOREIGN KEY ( id_jefe ) REFERENCES empleado( id_empleado );

___________

**INSERTANDO VALORES**

INSERT INTO producto (id_producto, nombre, id_gama, cantidad_stock, precio_venta, descripcion)
VALUES
('PROD001', 'Laptop HP', 'GAM0001', 50, 1200.00, 'Laptop HP con procesador i7 y 16GB de RAM.'),
('PROD002', 'Smartphone Samsung', 'GAM0002', 100, 500.00, 'Smartphone Samsung con pantalla AMOLED y cámara de alta resolución.'),
('PROD003', 'Monitor LG', 'GAM0003', 30, 250.00, 'Monitor LG de 24 pulgadas con resolución Full HD.'),
('PROD004', 'Lámpara de pie de cristal', 'GAM0004', 105, 400.00, 'Lámpara de pie con base de cristal tallado y pantalla de tela en tonos suaves.'),
('PROD005', 'Impresora Canon', 'GAM0005', 20, 150.00, 'Impresora Canon multifunción con conexión WiFi.'),
('PROD006', 'Auriculares Sony', 'GAM0006', 40, 180.00, 'Auriculares Sony con cancelación de ruido y alta fidelidad.'),
('PROD007', 'Alfombra persa de seda', 'GAM0004', 6, 1200.00, 'Alfombra persa tejida a mano con hilos de seda en diseños tradicionales y colores vivos.'),
('PROD008', 'Altavoz Bluetooth JBL', 'GAM0008', 90, 100.00, 'Altavoz Bluetooth JBL con batería de larga duración y resistencia al agua.'),
('PROD009', 'Reloj de pared antiguo', 'GAM0004', 101, 500.00, 'Reloj de pared antiguo con números romanos y mecanismo de péndulo visible.'),
('PROD010', 'Reproductor DVD Philips', 'GAM0010', 15, 70.00, 'Reproductor DVD Philips con salida HDMI y función de reproducción USB.'),
('PROD011', 'Router TP-Link', 'GAM0011', 50, 80.00, 'Router TP-Link de doble banda y velocidad de hasta 1200 Mbps.'),
('PROD012', 'Silla de Oficina', 'GAM0013', 35, 120.00, 'Silla de oficina ergonómica con respaldo ajustable y ruedas giratorias.'),
('PROD013', 'Escritorio IKEA', 'GAM0013', 10, 300.00, 'Escritorio IKEA con superficie amplia y cajones integrados.'),
('PROD014', 'Hervidor de Agua Moulinex', 'GAM0001', 45, 40.00, 'Hervidor de agua Moulinex con capacidad de 1.7 litros y apagado automático.');
INSERT INTO gama_producto (id_gama, nombre, descripcion_texto, id_imagen, id_html)
VALUES
('GAM0001', 'Electrónicos', 'Productos electrónicos de última generación.', NULL, NULL),
('GAM0002', 'Smartphones', 'Smartphones de diferentes marcas y especificaciones.', NULL, NULL),
('GAM0003', 'Monitores', 'Monitores de alta resolución y calidad de imagen.', NULL, NULL),
('GAM0004', 'Ornamentales', 'Productos decorativos y ornamentales para el hogar.', NULL, NULL),
('GAM0005', 'Impresoras', 'Impresoras multifunción y de calidad de impresión.', NULL, NULL),
('GAM0006', 'Auriculares', 'Auriculares con tecnología de cancelación de ruido.', NULL, NULL),
('GAM0008', 'Altavoces', 'Altavoces Bluetooth y de alta fidelidad.', NULL, NULL),
('GAM0010', 'Reproductores DVD', 'Reproductores DVD con diversas funciones.', NULL, NULL),
('GAM0011', 'Redes', 'Productos para redes y conectividad de alta velocidad.', NULL, NULL),
('GAM0013', 'Muebles de Oficina', 'Muebles ergonómicos para espacios de trabajo.', NULL, NULL);
INSERT INTO cliente (id_cliente, nombres, apellidos, fax, id_telefono, id_direccion, id_pais, id_ciudad, id_contacto, codigo_postal, limite_credito, id_empleado_ventas)
VALUES
(1, 'Juan', 'Pérez', '123456789', 'CO12345', 'BO0001', 'P001', 'C001', 'CNT001', '08001', 5000.00, 1),
(2, 'María', 'González', '987654321', 'AR54321', 'BU0002', 'P002', 'C003', 'CNT002', '28002', 8000.00, 2),
(3, 'José', 'López', '567890123', 'ES23456', 'FU0003', 'P003', 'C011', 'CNT003', '08002', 3000.00, 6),
(4, 'Ana', 'Martínez', '234567890', 'US67890', 'LO0004', 'P004', 'C007', 'CNT004', '28003', 7000.00, 4),
(5, 'Carlos', 'Rodríguez', '890123456', 'MX01234', 'CI0005', 'P005', 'C010', 'CNT005', '08003', 6000.00, 5),
(6, 'Laura', 'Sánchez', '345678901', 'CO34567', 'ME0006', 'P001', 'C002', 'CNT006', '28004', 4000.00, 11),
(7, 'David', 'Hernández', '012345678', 'ES34567', 'MA0007', 'P003', 'C006', 'CNT007', '08004', 9000.00, 7),
(8, 'Elena', 'Ruiz', '789012345', 'ES89012', 'MA0008', 'P003', 'C006', 'CNT008', '28005', 2000.00, 8),
(9, 'Javier', 'Díaz', '234567890', 'CO45678', 'ME0009', 'P002', 'C003', 'CNT009', '08005', 7500.00, 9),
(10, 'Sara', 'Fernández', '456789012', 'MX34567', 'ME0010', 'P005', 'C009', 'CNT010', '28006', 3500.00, 1);
INSERT INTO oficina (id_oficina, id_ciudad, id_pais, id_region, codigo_postal, id_telefono, id_direccion)
VALUES
('OF001', 'C001', 'P001', 'R001', '110011', '1234', 'DOF001'),
('OF002', 'C002', 'P001', 'R001', '110022', '0987', 'DOF002'),
('OF003', 'C003', 'P002', 'R002', '200011', '1122', 'DOF003'),
('OF004', 'C004', 'P002', 'R002', '200022', '9988', 'DOF004'),
('OF005', 'C005', 'P003', 'R003', '080011', '5544', 'DOF005'),
('OF006', 'C006', 'P003', 'R003', '080022', '6677', 'DOF006'),
('OF007', 'C007', 'P004', 'R004', '900011', '2233', 'DOF007'),
('OF008', 'C008', 'P004', 'R004', '900022', '7788', 'DOF008'),
('OF009', 'C009', 'P005', 'R005', '100011', '3344', 'DOF009'),
('OF010', 'C010', 'P005', 'R005', '100022', '8899', 'DOF010');
INSERT INTO direccion (id_direccion, tipo_via1, numero_via1, tipo_via2, numero_via2, barrio)
VALUES
('DOF001', 'Calle', 123, 'Avenida', 456, 'Centro'),
('DOF002', 'Carrera', 789, 'Diagonal', 321, 'Barrio Norte'),
('DOF003', 'Avenida', 555, 'Calle', 777, 'Poblado'),
('DOF004', 'Carrera', 111, 'Carrera', 222, 'La Floresta'),
('DOF005', 'Calle', 999, 'Diagonal', 888, 'El Prado'),
('DOF006', 'Avenida', 444, 'Calle', 333, 'La Candelaria'),
('DOF007', 'Carrera', 666, 'Avenida', 999, 'Miraflores'),
('DOF008', 'Calle', 222, 'Carrera', 444, 'San Antonio'),
('DOF009', 'Avenida', 888, 'Diagonal', 777, 'El Poblado'),
('DOF010', 'Carrera', 333, 'Calle', 555, 'La Paz');
INSERT INTO empleado (id_empleado, nombres, apellidos, email, id_oficina, id_jefe, puesto)
VALUES
(1, 'Juan', 'Pérez', 'juan.perez@example.com', 'OF001', 1, 'representante de ventas'),
(2, 'María', 'García', 'maria.garcia@example.com', NULL, 2, 'representante de ventas'),
(3, 'Carlos', 'López', 'carlos.lopez@example.com', 'OF003', 3, 'representante de ventas'),
(4, 'Ana', 'Martínez', 'ana.martinez@example.com', 'OF004', 4, 'representante de ventas'),
(5, 'Pedro', 'Gómez', 'pedro.gomez@example.com', 'OF005', 4, 'representante de ventas'),
(6, 'Laura', 'Ruiz', 'laura.ruiz@example.com', 'OF006', 3, 'representante de ventas'),
(7, 'Sofía', 'Hernández', 'sofia.hernandez@example.com', 'OF001', 2, 'representante de ventas'),
(8, 'Diego', 'Díaz', 'diego.diaz@example.com', 'OF002', 1, 'representante de ventas'),
(9, 'Elena', 'Sánchez', 'elena.sanchez@example.com', 'OF003', 2, 'representante de ventas'),
(10, 'Javier', 'Pérez', 'javier.perez@example.com', 'OF005', 0, 'CEO'),
(11, 'Luis', 'González', 'luis.gonzalez@example.com', 'OF010', 3, 'representante de ventas');

INSERT INTO telefono (id_telefono, numero_telefono, tipo_telefono)
VALUES
('1234', '(+57)12345678', 'fijo'),  -- Bogotá, Colombia
('0987', '(+57)19876543', 'fijo'),  -- Medellín, Colombia
('1122', '(+54)1122334455', 'fijo'),  -- Buenos Aires, Argentina
('9988', '(+54)1199887766', 'fijo'),  -- Córdoba, Argentina
('5544', '(+34)9355443322', 'fijo'),  -- Barcelona, España
('6677', '(+34)9966778899', 'fijo'),  -- Madrid, España
('2233', '(+1)32322334455', 'fijo'),  -- Los Angeles, USA
('7788', '(+1)30977889900', 'fijo'),  -- Miami, USA
('3344', '(+52)1333445566', 'fijo'),  -- Ciudad de México, México
('8899', '(+52)1888990011', 'fijo');  -- Mérida, México
INSERT INTO pais (id_pais, nombre)
VALUES
('P001', 'Colombia'),
('P002', 'Argentina'),
('P003', 'España'),
('P004', 'USA'),
('P005', 'México');
INSERT INTO region (id_region, id_pais, nombre)
VALUES
('R001', 'P001', 'Andina'),
('R002', 'P001', 'Caribe'),
('R003', 'P002', 'Pampa'),
('R004', 'P002', 'Patagonia'),
('R005', 'P003', 'Cataluña'),
('R006', 'P003', 'Andalucía'),
('R007', 'P004', 'California'),
('R008', 'P004', 'Florida'),
('R009', 'P005', 'Ciudad de México'),
('R010', 'P005', 'Yucatán');
INSERT INTO ciudad (id_ciudad, id_region, nombre)
VALUES
('C001', 'R001', 'Bogotá'),
('C002', 'R001', 'Medellín'),
('C003', 'R002', 'Buenos Aires'),
('C004', 'R002', 'Córdoba'),
('C005', 'R003', 'Barcelona'),
('C006', 'R003', 'Madrid'),
('C007', 'R004', 'Los Angeles'),
('C008', 'R004', 'Miami'),
('C009', 'R005', 'Ciudad de México'),
('C010', 'R005', 'Mérida'),
('C011', 'R003', 'Fuenlabrada');
INSERT INTO pago (id_pago, id_cliente, forma_pago, fecha_pago, total)
VALUES
('PAG0001', 1, 'paypal', '2008-02-15', 150.00),
('PAG0002', 2, 'efectivo','2010-04-20', 300.50),
('PAG0003', 3, 'paypal','2008-06-10', 450.75),
('PAG0004', 7, 'tarjeta credito','2012-12-05', 200.25),
('PAG0005', 5, 'efectivo','2009-10-15', 550.00),
('PAG0006', 3, 'paypal','2008-11-30', 100.00),
('PAG0007', 7, 'tarjeta debito','2013-08-25', 350.80),
('PAG0008', 3, 'paypal','2018-07-18', 430.00),
('PAG0009', 9, 'tarjeta credito','2008-09-05', 275.60),
('PAG0010', 7, 'efectivo','2015-03-12', 150.00);

INSERT INTO estado_pedido (id_estado, nombre)
VALUES
(1, 'En proceso'),
(2, 'Pendiente de pago'),
(3, 'Enviado'),
(4, 'Entregado'),
(5, 'entregado con retraso'),
(6, 'Cancelado');

INSERT INTO pedido (id_pedido, id_cliente, fecha_pedido, fecha_esperada, fecha_entrega, id_estado, comentarios)
VALUES
(101, 3, '2023-03-05', '2023-03-10', '2023-03-12', 5, 'Pedido entregado con retraso'),
(102, 7, '2023-03-08', '2023-03-12', '2023-03-10', 4, 'Cliente satisfecho con la entrega'),
(103, 9, '2023-03-10', '2023-03-15', NULL, 1, 'Pedido en proceso de preparación'),
(104, 1, '2023-03-12', '2023-03-18', '2023-03-20', 5, 'Pedido entregado con retraso'),
(105, 2, '2023-03-15', '2023-03-20', NULL, 6, 'Pedido cancelado'),
(106, 6, '2023-03-18', '2023-03-25', '2023-03-24', 4, 'Entrega realizada con éxito'),
(107, 5, '2023-03-20', '2023-03-25', NULL, 1, 'Pedido en proceso de preparación'),
(108, 8, '2023-01-22', '2023-01-28', '2023-01-26', 2, 'Pedido pendiente de pago'),
(109, 4, '2023-03-25', '2023-03-30', '2023-04-02', 5, 'Pedido entregado con retraso'),
(110, 1, '2023-03-28', '2023-04-03', '2023-04-03', 4, 'Entrega realizada con éxito'),
(111, 3, '2023-03-30', '2023-04-05', NULL, 3, 'Pedido enviado al cliente'),
(112, 6, '2023-04-02', '2023-04-08', '2023-04-06', 4, 'Entrega exitosa'),
(113, 2, '2022-01-05', '2022-01-10', '2022-01-10', 4, 'Entrega puntual'),
(114, 3, '2008-05-10', '2008-05-15', NULL, 6, 'Pedido cancelado por el cliente'),
(115, 8, '2009-08-20', '2009-08-25', NULL, 2, 'Pedido pendiente de pago'),
(116, 2, '2008-01-05', '2008-01-10', '2023-01-10', 4, 'Entrega realizada con éxito'),
(117, 7, '2020-01-15', '2020-01-20', '2020-01-20', 4, 'Entrega realizada con éxito');
INSERT INTO detalle_pedido (id_pedido, id_producto, cantidad, precio_unidad, numero_linea)
VALUES
(101, 'PROD002', 2, 500.00, 1),
(101, 'PROD008', 1, 100.00, 2),
(102, 'PROD013', 3, 25.00, 1),
(102, 'PROD003', 1, 250.00, 2),
(103, 'PROD009', 4, 500.00, 1),
(103, 'PROD001', 4, 1200.00, 2),
(104, 'PROD012', 2, 120.00, 1),
(104, 'PROD004', 1, 400.00, 2),
(105, 'PROD006', 3, 180.00, 1),
(107, 'PROD005', 2, 150.00, 2);

___________

**EXPLICACION DE LA FORMALIZACION**

**Tabla cliente_contacto:** Esta tabla se creó para manejar la relación de muchos a muchos entre clientes y contactos. Un cliente puede tener múltiples contactos y, a su vez, un contacto puede estar asociado con varios clientes. Esto evita la redundancia de datos al no tener que repetir información de contacto para cada cliente y garantiza la integridad de los datos al mantener esta relación de manera estructurada. Cumple con la 3NF ya que elimina la dependencia transitiva al separar la información de contacto en una tabla aparte.
**Tabla producto_proveedor:** Aquí se maneja la relación de muchos a muchos entre productos y proveedores. Un producto puede ser suministrado por varios proveedores, y un proveedor puede suministrar varios productos. Al separar esta relación en una tabla independiente, se evita la duplicación de información y se facilita la gestión de la asociación entre productos y proveedores.
**Tablas pais, region y ciudad:** Estas tablas se crearon para organizar jerárquicamente la información geográfica. Un país puede tener varias regiones y cada región puede tener múltiples ciudades. Este diseño cumple con la 2NF al eliminar las dependencias parciales y la 3NF al eliminar las dependencias transitivas.
**Tabla telefono:** Esta tabla se utiliza para almacenar información sobre los teléfonos, incluyendo su tipo (móvil, fijo, etc.). Al separar esta información en una tabla aparte, se evita la repetición de datos y se facilita la gestión de los números de teléfono asociados con clientes, empleados u otras entidades. Cumple con la 1NF al almacenar un solo valor en cada celda.
**Tabla empleado_jefe:** Aquí se maneja la relación de muchos a uno entre empleados y sus jefes. Un empleado puede tener un jefe, pero un jefe puede tener varios empleados bajo su supervisión. Al separar esta relación en una tabla independiente, se simplifica la gestión de la jerarquía organizacional y se evita la redundancia de información. Cumple con la 2NF al eliminar las dependencias parciales y la 3NF al eliminar las dependencias transitivas.

_______________________________

**I)	CONSULTAS SOBRE UNA TABLA**

1. SELECT o.id_oficina, o.id_ciudad, c.nombre
   FROM oficina AS o, ciudad AS c
   WHERE o.id_ciudad = c.id_ciudad

   ![image](/images/1.png)
   Ciudades donde hay oficinas

2. SELECT p.nombre AS pais, c.nombre AS ciudad, o.id_oficina, t.numero_telefono
   FROM oficina AS o
   JOIN pais as p ON o.id_pais = p.id_pais
   JOIN ciudad as c ON o.id_ciudad = c.id_ciudad
   JOIN telefono as t on o.id_telefono = t.id_telefono
   WHERE p.nombre = 'España'

   ![image](/images/2.png)
   Teléfonos de las oficinas de España

3. SELECT e.nombres, e.apellidos, e.email, e.id_jefe
   FROM empleado AS e
   WHERE e.id_jefe = 7

   ![image](/images/3.png)
   Empleados cuyo jefe tiene un id_jefe igual a 7

4. SELECT e.nombres, e.apellidos, e.email, e.puesto
   FROM empleado AS e
   WHERE puesto = 'CEO'

   ![image](/images/4.png)

   Informacion del jefe de la empresa

5. SELECT e.nombres, e.apellidos, e.puesto
   FROM empleado AS e
   WHERE puesto != 'representante de ventas'

   ![image](/images/5.png)

   Empleados que no son representante de ventas

6. SELECT c.nombres, c.apellidos, p.nombre AS pais
   FROM cliente AS c
   JOIN pais AS p ON c.id_pais = p.id_pais
   WHERE p.nombre = 'España'

​	![image](/images/6.png)
​	Nombre de los clientes de España

7. SELECT ep.id_estado, ep.nombre AS 'Nombre de estado'
   FROM estado_pedido AS ep

   ![image](/images/7.png)

   Distintos estados por los que puede pasar un pedido

8. SELECT DISTINCT p.id_cliente, p.fecha_pago
   FROM pago AS p
   WHERE YEAR(fecha_pago) = 2008; *utilizando la funcion YEAR. Resultado en tipo de dato númerico*

   ______________________________________

   SELECT DISTINCT p.id_cliente, p.fecha_pago
   FROM pago as p
   WHERE DATE_FORMAT(fecha_pago, '%Y') = '2008'; *utilizando la funcion DATE_FORMAT. Formatea la columna para obtener solo el año en formato cadena*

   __________________

   SELECT DISTINCT p.id_cliente, p.fecha_pago
   FROM pago as p
   WHERE fecha_pago >= '2008-01-01' AND fecha_pago <= '2008-12-31'; *Sin utilizar ninguna funcion*

   ![image](/images/8.png)

   Clientes con algun pago en el año 2008

9. SELECT p.id_pedido, p.id_cliente, p.fecha_esperada, p.fecha_entrega
   FROM pedido as p
   WHERE p.fecha_entrega > p.fecha_esperada;

   ![image](/images/9.png)
   Pedidos que no han sido entregados a tiempo

10. SELECT id_pedido, id_cliente, fecha_esperada, fecha_entrega
    FROM pedido
    WHERE fecha_entrega <= ADDDATE(fecha_esperada, -2); *utilizando ADDDATE*

    _________________

    SELECT id_pedido, id_cliente, fecha_esperada, fecha_entrega 

    FROM pedido 

    WHERE DATEDIFF(fecha_esperada, fecha_entrega) >= 2; *utilizando DATEDIFF*

    __________

    SELECT p.id_pedido, p.id_cliente, p.fecha_esperada, p.fecha_entrega
    FROM pedido AS p
    WHERE fecha_entrega <= fecha_esperada - INTERVAL 2 DAY; *utilizando INTERVAL*

    ![image](/images/10.png)

    pedidos entregados con al menos dos dias de anticipacion

11. SELECT p.id_pedido, p.id_cliente, p.fecha_esperada, p.fecha_entrega, ep.nombre AS estado
    FROM pedido as p
    JOIN estado_pedido AS ep ON ep.id_estado = p.id_estado
    WHERE p.id_estado = 6 AND YEAR(fecha_pedido) = 2009;

    ![image](/images/11.png)

    Pedidos que fueron cancelados en 2009

12. SELECT p.id_pedido, p.id_cliente, p.fecha_esperada, p.fecha_entrega, ep.nombre AS estado
    FROM pedido as p
    JOIN estado_pedido AS ep ON ep.id_estado = p.id_estado
    WHERE p.id_estado = 4 AND DATE_FORMAT(fecha_entrega, '%m') = '01';

    ![image](/images/12.png)

    Pedidos que fueron entregados en el mes de enero de cualquier año

13. SELECT p.id_pago, p.forma_pago, p.fecha_pago, p.total
    FROM pago AS p
    WHERE p.forma_pago = 'paypal' AND YEAR(p.fecha_pago) = 2008

    ORDER BY p.total DESC;

    ![image](/images/13.png)

    Pagos realizados en 2008 mediante paypal. Ordenados segun dinero ingresado

14. SELECT DISTINCT p.forma_pago AS 'Formas de pago'
    FROM pago AS p

    ![image](/images/14.png)
    Formas de pago de la tabla pago

15. SELECT p.id_producto, p.nombre, g.nombre AS gama, p.cantidad_stock, p.precio_venta
    FROM producto AS p
    JOIN gama_producto AS g ON p.id_gama = g.id_gama
    WHERE g.nombre = 'Ornamentales' AND p.cantidad_stock > 100

    ![image](/images/15.png)

    Productos de la gama Ornamentales con mas de 100 unidades en stock

16. SELECT c.id_cliente, c.nombres, c.apellidos, ci.nombre, c.id_empleado_ventas
    FROM cliente AS c
    JOIN ciudad AS ci ON c.id_ciudad = ci.id_ciudad
    WHERE ci.nombre = 'Madrid' AND (c.id_empleado_ventas = 11 OR c.id_empleado_ventas = 30)

    ![image](/images/16.png)

**II)  CONSULTAS MULTITABLA (Composicion Interna)**

1. SELECT c.id_cliente, c.nombres AS 'Nombre de cliente', e.nombres AS 'Nombre Rep. Ventas', e.apellidos AS 'Apellidos Rep.Ventas' 
   FROM cliente AS c
   JOIN empleado AS e ON e.id_empleado = c.id_empleado_ventas

   ![image](/images/17.png)
   Clientes y los nombres y apellidos de su representante de ventas

2. SELECT c.id_cliente, c.nombres AS 'Nombre de cliente', e.nombres AS 'Nombre Rep. Ventas', p.id_pago
   FROM cliente AS c
   JOIN empleado AS e ON e.id_empleado = c.id_empleado_ventas
   JOIN pago AS p ON c.id_cliente = p.id_cliente

   ![image](/images/18.png)

   Clientes que han realizado pago y su representante de ventas

3. SELECT c.id_cliente, c.nombres AS 'Nombre de cliente', e.nombres AS 'Nombre Rep. Ventas'
   FROM cliente AS c
   JOIN empleado AS e ON e.id_empleado = c.id_empleado_ventas
   LEFT JOIN pago AS p ON c.id_cliente = p.id_cliente
   WHERE p.id_pago IS NULL

   ![image](/images/19.png)

   Clientes que NO han realizado pago y su representante de ventas

4. SELECT c.id_cliente, c.nombres AS 'Nombre de cliente', p.id_pago, e.nombres AS 'Nombre Rep. Ventas', o.id_oficina, ci.nombre AS 'Ciudad'
   FROM cliente AS c
   JOIN empleado AS e ON e.id_empleado = c.id_empleado_ventas
   JOIN oficina AS o ON o.id_oficina = e.id_oficina
   JOIN ciudad AS ci ON o.id_ciudad = ci.id_ciudad
   JOIN pago AS p ON c.id_cliente = p.id_cliente

   ![image](/images/20.png)
   Clientes con pagos, su representante de ventas y la ciudad de la oficina a la que pertenecen

5. SELECT c.id_cliente, c.nombres AS 'Nombre de cliente', p.id_pago, e.nombres AS 'Nombre Rep. Ventas', o.id_oficina, ci.nombre AS 'Ciudad'
   FROM cliente AS c
   JOIN empleado AS e ON e.id_empleado = c.id_empleado_ventas
   JOIN oficina AS o ON o.id_oficina = e.id_oficina
   JOIN ciudad AS ci ON o.id_ciudad = ci.id_ciudad
   LEFT JOIN pago AS p ON c.id_cliente = p.id_cliente
   WHERE p.id_pago IS NULL

   ![image](/images/21.png)

   Clientes SIN pagos, su representante de ventas y la ciudad de la oficina a la que pertenecen

6. SELECT o.id_oficina, d.tipo_via1, d.numero_via1, d.tipo_via2, d.numero_via2, d.barrio, ci.nombre AS 'Ciudad'
   FROM direccion AS d
   JOIN oficina AS o ON o.id_direccion = d.id_direccion
   JOIN empleado AS e ON e.id_oficina = o.id_oficina
   JOIN cliente AS c ON e.id_empleado = c.id_empleado_ventas
   JOIN ciudad AS ci ON c.id_ciudad = ci.id_ciudad
   WHERE ci.nombre = 'Fuenlabrada';
   ![image](/images/22.png)
   Direccion de oficinas que tienen clientes en Fuenlabrada

7. SELECT c.id_cliente, c.nombres AS 'Nombre de cliente', e.nombres AS 'Nombre Rep. Ventas', o.id_oficina, ci.nombre AS 'Ciudad_oficina'
   FROM cliente AS c
   JOIN empleado AS e ON e.id_empleado = c.id_empleado_ventas
   JOIN oficina AS o ON o.id_oficina = e.id_oficina
   JOIN ciudad AS ci ON o.id_ciudad = ci.id_ciudad

   ![image](/images/23.png)

   Clientes, sus representantes de ventas y la ciudad de la oficina del representante

8. SELECT c.id_cliente, c.nombres, c.apellidos, p.id_pedido, ep.nombre AS 'Estado de pedido'
   FROM cliente AS c
   JOIN pedido AS p ON p.id_cliente = c.id_cliente
   JOIN estado_pedido AS ep ON ep.id_estado = p.id_estado
   WHERE p.id_estado = 5

   ![image](/images/26.png)

   Clientes con pedidos entregados con retraso

9. SELECT gp.id_gama, gp.nombre AS gama, c.nombres AS 'Adquirido por', c.id_cliente
   FROM gama_producto as gp
   JOIN producto as p ON p.id_gama = gp.id_gama
   JOIN detalle_pedido AS dp ON dp.id_producto = p.id_producto
   JOIN pedido AS pe ON pe.id_pedido = dp.id_pedido
   JOIN cliente AS c ON c.id_cliente = pe.id_cliente

   ![image](/images/27.png)
   Gamas de producto que ha comprado cada cliente

**III)	CONSULTAS MULTITABLA (Composicion Externa)**

1. SELECT c.id_cliente, c.nombres, c.apellidos, ci.nombre AS ciudad, p.id_pago
   FROM cliente AS c
   LEFT JOIN pago AS p ON c.id_cliente = p.id_cliente
   JOIN ciudad AS ci ON c.id_ciudad = ci.id_ciudad
   WHERE p.id_pago IS NULL

   ![image](/images/28.png)

2. SELECT c.id_cliente, c.nombres, c.apellidos, ci.nombre AS ciudad, p.id_pedido
   FROM cliente AS c
   LEFT JOIN pedido AS p ON c.id_cliente = p.id_cliente
   JOIN ciudad AS ci ON c.id_ciudad = ci.id_ciudad
   WHERE p.id_pedido IS NULL

   ![image](/images/29.png)

   Clientes que no han realizado ningun pedido

3. SELECT DISTINCT c.id_cliente, c.nombres, c.apellidos, ci.nombre AS ciudad, p.id_pedido, pa.id_pago
   FROM cliente AS c
   LEFT JOIN pedido AS p ON c.id_cliente = p.id_cliente
   LEFT JOIN pago AS pa ON c.id_cliente = pa.id_cliente
   JOIN ciudad AS ci ON c.id_ciudad = ci.id_ciudad
   WHERE pa.id_pago IS NULL AND p.id_pedido IS NULL

   ![image](/images/30.png)

   Clientes que no han realizado ningun pago y ningun pedido

4. SELECT e.id_empleado, e.nombres, e.apellidos, e.id_oficina
   FROM oficina AS o
   RIGHT JOIN empleado AS e ON e.id_oficina = o.id_oficina
   WHERE e.id_oficina IS NULL

   ![image](/images/31.png)

   Empleados que no tienen una oficina asociada

5. SELECT e.id_empleado, e.nombres, e.apellidos, c.id_empleado_ventas AS 'Cliente Asociado'
   FROM empleado AS e
   LEFT JOIN cliente AS c ON e.id_empleado = c.id_empleado_ventas
   WHERE c.id_empleado_ventas IS NULL

   ![image](/images/32.png)

   Empleados que no tienen un cliente asociado

6. SELECT e.id_empleado, e.nombres, e.apellidos, c.id_empleado_ventas AS 'Cliente Asociado', o.id_oficina, o.id_ciudad, o.id_direccion 
   FROM empleado AS e
   LEFT JOIN cliente AS c ON e.id_empleado = c.id_empleado_ventas
   LEFT JOIN oficina AS o ON o.id_oficina = e.id_oficina
   WHERE c.id_empleado_ventas IS NULL

   ![image](/images/33.png)

   Empleados que no tienen un cliente asociado y los datos de su oficina

7. SELECT e.id_empleado, e.nombres, e.apellidos, e.id_oficina,
          CASE WHEN o.id_oficina IS NULL THEN 'No tiene oficina asociada' ELSE 'Tiene oficina asociada' END AS estado_oficina,
          CASE WHEN c.id_cliente IS NULL THEN 'No tiene cliente asociado' ELSE 'Tiene cliente asociado' END AS estado_cliente
   FROM empleado e
   LEFT JOIN oficina o ON e.id_oficina = o.id_oficina
   LEFT JOIN cliente c ON e.id_empleado = c.id_empleado_ventas
   WHERE o.id_oficina IS NULL OR c.id_cliente IS NULL;

   ![image](/images/34.png)

    empleados que no tienen una oficina asociada y los que no tienen un cliente asociado.

8. SELECT p.id_producto, p.nombre, p.id_gama, p.precio_venta, id_pedido
   FROM producto AS p
   LEFT JOIN detalle_pedido as dp ON dp.id_producto = p.id_producto
   WHERE id_pedido IS NULL

   ![image](/images/35.png)
   Productos que nunca han sido listados en un pedido

9. SELECT p.id_producto, p.nombre, p.descripcion, gp.id_imagen
   FROM producto AS p
   LEFT JOIN detalle_pedido as dp ON dp.id_producto = p.id_producto
   JOIN gama_producto AS gp ON p.id_gama = gp.id_gama
   WHERE id_pedido IS NULL
   ![image](/images/36.png)
   Productos que nunca han sido listados en un pedido

11. SELECT DISTINCT c.id_cliente, c.nombres, c.apellidos
    FROM cliente c
    LEFT JOIN pedido p ON c.id_cliente = p.id_cliente
    LEFT JOIN pago pg ON c.id_cliente = pg.id_cliente
    WHERE p.id_pedido IS NOT NULL AND pg.id_pago IS NULL;

    ![image](/images/38.png)
    clientes que han realizado algún pedido pero no han realizado ningún pago

12. SELECT e.id_empleado, e.nombres AS nombre_empleado, e.apellidos AS apellidos_empleado,
           ej.id_jefe, ej.nombres AS nombre_jefe, ej.apellidos AS apellidos_jefe
    FROM empleado e
    LEFT JOIN cliente c ON e.id_empleado = c.id_empleado_ventas
    LEFT JOIN empleado_jefe ej ON e.id_jefe = ej.id_jefe
    WHERE c.id_cliente IS NULL;
    ![image](/images/39.png)
    empleados que no tienen clientes asociados y el nombre de su jefe asociado

**IV)	CONSULTAS RESUMEN**

1. SELECT COUNT(id_empleado) AS total_empleados
   FROM empleado;
   ![image](/images/40.png)

2. SELECT p.nombre AS nombre_pais, COUNT(c.id_cliente) AS total_clientes
   FROM cliente c
   JOIN pais p ON c.id_pais = p.id_pais
   GROUP BY p.nombre;
   ![image](/images/41.png)

3. SELECT ROUND(AVG(p.total), 2) AS 'Promedio de pago 2009'
   FROM pago AS p
   WHERE YEAR(p.fecha_pago) = 2009
   ![image](/images/42.png)
   Promedio de pago en 2009

4. SELECT ep.nombre AS estado_pedido, COUNT(p.id_pedido) AS total_pedidos
   FROM pedido p
   JOIN estado_pedido ep ON p.id_estado = ep.id_estado
   GROUP BY ep.nombre
   ORDER BY total_pedidos DESC;
   ![image](/images/43.png)
   Total de pedidos segun el estado en que se encuentran. Ordenados de manera descendente

5. SELECT MAX(precio_venta) AS 'precio mas caro', MIN(precio_venta) AS 'precio mas barato'
   FROM producto;

   ![image](/images/44.png)
   El precio mas caro y el mas barato de los productos

6. SELECT COUNT(id_cliente) AS numero_clientes
   FROM cliente;

   ![image](/images/45.png)
   Numero de clientes de la empresa

7. SELECT COUNT(c.id_cliente) AS 'clientes_ de madrid'
   FROM cliente c
   JOIN ciudad ci ON c.id_ciudad = ci.id_ciudad
   WHERE ci.nombre = 'Madrid';

   ![image](/images/46.png)
   Numero de clientes de Madrid de la empresa 

8. SELECT ci.nombre AS ciudad, COUNT(c.id_cliente) AS numero_clientes
   FROM cliente c
   JOIN ciudad ci ON c.id_ciudad = ci.id_ciudad
   WHERE ci.nombre LIKE 'M%'
   GROUP BY ci.nombre;
   ![image](/images/47.png)
   Total de clientes en ciudades que empiezan por M

9. SELECT e.nombres, COUNT(c.id_cliente) AS 'clientes atendidos'
   FROM empleado e
   LEFT JOIN cliente c ON e.id_empleado = c.id_empleado_ventas
   GROUP BY e.nombres;
   ![image](/images/48.png)
   Clientes atendidos por cada uno de los representantes de ventas

10. SELECT COUNT(id_cliente) AS 'clientes sin representante'
    FROM cliente
    WHERE id_empleado_ventas IS NULL;
    ![image](/images/49.png)
    Clientes sin representande de ventas

11. SELECT c.nombres, c.apellidos, MIN(p.fecha_pago) AS 'primer pago', MAX(p.fecha_pago) AS 'ultimo pago'
    FROM cliente AS c
    LEFT JOIN pago AS p ON c.id_cliente = p.id_cliente
    GROUP BY c.nombres, c.apellidos;
    ![image](/images/50.png)
    Fecha del primer y utlimo pago de cada cliente

12. SELECT id_pedido, COUNT(id_producto) AS 'productos diferentes'
    FROM detalle_pedido
    GROUP BY id_pedido;
    ![image](/images/51.png)
    Diferentes productos que hay en cada pedido

13. SELECT id_pedido, SUM(cantidad) AS 'total de productos'
    FROM detalle_pedido
    GROUP BY id_pedido;
    ![image](/images/52.png)

14. SELECT dp.id_producto, p.nombre AS 'producto', SUM(dp.cantidad) AS total_unidades_vendidas
    FROM detalle_pedido AS dp
    JOIN producto as p ON p.id_producto = dp.id_producto
    GROUP BY id_producto
    ORDER BY total_unidades_vendidas DESC
    LIMIT 20;

    ![image](/images/53.png)

15. SELECT 
        ROUND(SUM(d.cantidad * p.precio_venta),2) AS 'base imponible',
        ROUND(SUM(d.cantidad * p.precio_venta) * 0.21,2) AS iva,
        ROUND(SUM(d.cantidad * p.precio_venta),2) + ROUND(SUM(d.cantidad * p.precio_venta) * 0.21,2) AS 'total facturado'
    FROM detalle_pedido AS d
    JOIN producto AS p ON d.id_producto = p.id_producto;

    ![image](/images/54.png)

16. SELECT dp.id_producto AS 'codigo producto', SUM(dp.cantidad) AS 'total unidades vendidas'
    FROM detalle_pedido AS dp
    GROUP BY dp.id_producto;

    ![image](/images/55.png)

17. SELECT dp.id_producto, SUM(dp.cantidad) AS 'total_unidades_vendidas'
    FROM detalle_pedido dp
    WHERE dp.id_producto LIKE 'OR%'
    GROUP BY dp.id_producto;
    No hay productos cuyo id_producto empiece por OR

18. SELECT p.nombre AS producto, SUM(dp.cantidad) AS 'unidades vendidas', ROUND(SUM(dp.cantidad * p.precio_venta),2) AS total_facturado, ROUND(SUM(dp.cantidad * p.precio_venta * 1.21),2) AS 'total facturado con IVA'
    FROM detalle_pedido AS dp
    JOIN producto AS p ON dp.id_producto = p.id_producto
    GROUP BY p.nombre
    HAVING total_facturado > 3000;
    ![image](/images/56.png)

19. SELECT YEAR(fecha_pago) AS 'Año', SUM(total) AS 'Facturacion del año'
    FROM pago AS p
    GROUP BY YEAR(fecha_pago)
    ORDER BY YEAR(fecha_pago) ASC;
    ![image](/images/57.png)

**V)	CONSULTAS VARIADAS**

1. SELECT c.nombres AS cliente , COUNT(p.id_pedido) AS 'pedidos realizados'
   FROM cliente c
   LEFT JOIN pedido p ON c.id_cliente = p.id_cliente
   GROUP BY c.nombres;
   ![image](/images/58.png)
2. SELECT c.nombres AS cliente, COALESCE(SUM(pa.total), 0) AS 'total pagado'
   FROM cliente AS c
   LEFT JOIN pago AS pa ON c.id_cliente = pa.id_cliente
   GROUP BY c.nombres;
   ![image](/images/59.png)
3. SELECT c.nombres, c.apellidos, p.id_pedido, YEAR(p.fecha_pedido) AS 'Año'
   FROM cliente AS c
   JOIN pedido AS p ON c.id_cliente = p.id_cliente
   WHERE YEAR(p.fecha_pedido) = 2008
   ORDER BY c.nombres ASC;
   ![image](/images/60.png)
4. SELECT 
   c.nombres AS 'nombre cliente', CONCAT(e.nombres, ' ', e.apellidos) AS 'nombre representante', t.numero_telefono AS 'telefono de oficina'
   FROM  cliente AS c
   LEFT JOIN  empleado AS e ON c.id_empleado_ventas = e.id_empleado
   LEFT JOIN  oficina AS o ON e.id_oficina = o.id_oficina
   LEFT JOIN  telefono AS t ON o.id_telefono = t.id_telefono
   WHERE  c.id_cliente NOT IN (SELECT id_cliente FROM pago)
   ![image](/images/61.png)
5. SELECT 
   c.nombres AS cliente,
   CONCAT(e.nombres, ' ', e.apellidos) AS 'nombre representante',
   ci.nombre AS ciudad
   FROM  cliente AS c LEFT 
   JOIN  empleado AS e ON c.id_empleado_ventas = e.id_empleado
   LEFT JOIN  oficina AS o ON e.id_oficina = o.id_oficina
   LEFT JOIN  ciudad AS ci ON o.id_ciudad = ci.id_ciudad;
   ![image](/images/62.png)
6. SELECT 
       e.nombres AS 'nombre',
       e.apellidos AS 'apellidos',
       e.puesto AS 'puesto',
       t.numero_telefono AS 'telefono'
   FROM  empleado AS e
   JOIN oficina AS o ON e.id_oficina = o.id_oficina
   JOIN telefono AS t ON o.id_telefono = t.id_telefono
   WHERE  e.id_empleado NOT IN (SELECT id_empleado_ventas FROM cliente);
   ![image](/images/63.png)
   empleados que no son representante de ventas de ningún cliente
7. SELECT ci.nombre AS ciudad, COUNT(e.id_empleado) AS 'numero de empleados'
   FROM ciudad AS ci
   LEFT JOIN oficina AS o ON ci.id_ciudad = o.id_ciudad
   LEFT JOIN empleado AS e ON o.id_oficina = e.id_oficina
   GROUP BY ci.nombre;
   ![image](/images/64.png)



