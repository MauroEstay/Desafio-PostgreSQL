## Desafío - PostgreSQL

En este desafío trabajaremos con las diferentes queries desde el terminal de postgres.

Para desarrollar esta desafío, tendrán que anotar cada una de las queries que utilizaron en un archivo txt o md y subir este archivo a la plataforma.

Deben también ingresar, al final de este archivo, el nombre de los integrantes del grupo que participaron en el desarrollo. (Cada integrante debe subir el archivo).

Crear base de datos en base a los requerimientos enviados por el cliente.

**1)** Crear una base de datos llamada call_list

	CREATE DATABASE call_list;

**2)** En la base de datos recien creada, crear la tabla users con los campos id (clave primaria), first_name, email.
	

	        \c call_list
    	CREATE TABLE users(id SERIAL PRIMARY KEY,first_name VARCHAR(64), email VARCHAR(32));
    	\d users
    
                                         Table "public.users"
       Column   |         Type          | Collation | Nullable |              Default              
    ------------+-----------------------+-----------+----------+-----------------------------------
     id         | integer               |           | not null | nextval('users_id_seq'::regclass)
     first_name | character varying(64) |           |          | 
     email      | character varying(32) |           |          | 
    Indexes:
        "users_pkey" PRIMARY KEY, btree (id)

**3)** Ingresar un usuario, llamado Carlos (el resto de los datos deben inventarlos).

	INSERT INTO users (first_name, email) VALUES ('Carlos', 'carlos@dl.com');

**4)** Ingresar un usuario, llamada Laura (el resto de los datos deben inventarlos).

	INSERT INTO users (first_name, email) VALUES ('Laura', 'laura@dl.com');

**5)** Crear una tabla llamada calls con los campos id (clave primaria), phone, date, user_id (foreign key relacionado a users) y length (para guardar la duración de la llamada) como un número entero.

	CREATE TABLE calls(id SERIAL PRIMARY KEY, 
				phone NUMERIC, 
				date DATE, 
				user_id INTEGER, 
				length INTEGER);

	ALTER TABLE calls ADD FOREIGN KEY (user_id) REFERENCES users (id);
				

**6)** Agregar a la tabla users el campo last_name.

 	

    ALTER TABLE users ADD COLUMN last_name VARCHAR(30);
    \d users
    
                                         Table "public.users"
       Column   |         Type          | Collation | Nullable |              Default              
    ------------+-----------------------+-----------+----------+-----------------------------------
     id         | integer               |           | not null | nextval('users_id_seq'::regclass)
     first_name | character varying(64) |           |          | 
     email      | character varying(32) |           |          | 
     last_name  | character varying(30) |           |          | 
    Indexes:
        "users_pkey" PRIMARY KEY, btree (id)
    Referenced by:
        TABLE "calls" CONSTRAINT "calls_user_id_fkey" FOREIGN KEY (user_id) REFERENCES users(id)

**7)** Modelar la base de datos (agregar print de pantalla [utilizar https://www.draw.io/]).

![](BD_call_list.png)
**8)** Ingresar el apellido del usuario Carlos.

	UPDATE users SET last_name ='Cabezas' WHERE id=1;

	select * from users;

	 id | first_name |     email     | last_name 
	----+------------+---------------+-----------
	  2 | Laura      | laura@dl.com  | 
	  1 | Carlos     | carlos@dl.com | Cabezas



**9)** Ingresar el apellido del usuario Laura.

	UPDATE users SET last_name ='Prieto' WHERE id=2;

	select * from users;

	 id | first_name |     email     | last_name 
	----+------------+---------------+-----------
	  1 | Carlos     | carlos@dl.com | Cabezas
	  2 | Laura      | laura@dl.com  | Prieto



**10)** Ingresar 6 llamadas asociadas al usuario Laura.
	

            INSERT INTO calls(phone, date, user_id, length) VALUES (985632145, NOW(), 2, 68);
    	INSERT INTO calls(phone, date, user_id, length) VALUES (156582678, NOW(), 2, 551);
    	INSERT INTO calls(phone, date, user_id, length) VALUES (455852125, NOW(), 2, 151);
    	INSERT INTO calls(phone, date, user_id, length) VALUES (182828825, NOW(), 2, 15);
    	INSERT INTO calls(phone, date, user_id, length) VALUES (828282885, NOW(), 2, 156);
    	INSERT INTO calls(phone, date, user_id, length) VALUES (828228282, NOW(), 2, 854);
    
    	select * from calls;
    
    	 id |   phone   |    date    | user_id | length 
    	----+-----------+------------+---------+--------
    	  1 | 985632145 | 2019-08-02 |       2 |     68
    	  2 | 156582678 | 2019-08-02 |       2 |    551
    	  3 | 455852125 | 2019-08-02 |       2 |    151
    	  4 | 182828825 | 2019-08-02 |       2 |     15
    	  5 | 828282885 | 2019-08-02 |       2 |    156
    	  6 | 828228282 | 2019-08-02 |       2 |    854
    	

**11)** Ingresar 4 llamadas asociadas al usuario Carlos.

	INSERT INTO calls(phone, date, user_id, length) VALUES (922445525, NOW(), 1, 963);
	INSERT INTO calls(phone, date, user_id, length) VALUES (156585478, NOW(), 1, 851);
	INSERT INTO calls(phone, date, user_id, length) VALUES (452265425, NOW(), 1, 15);
	INSERT INTO calls(phone, date, user_id, length) VALUES (122556525, NOW(), 1, 254);

	select * from calls;

	 id |   phone   |    date    | user_id | length 
	----+-----------+------------+---------+--------
	  1 | 985632145 | 2019-08-02 |       2 |     68
	  2 | 156582678 | 2019-08-02 |       2 |    551
	  3 | 455852125 | 2019-08-02 |       2 |    151
	  4 | 182828825 | 2019-08-02 |       2 |     15
	  5 | 828282885 | 2019-08-02 |       2 |    156
	  6 | 828228282 | 2019-08-02 |       2 |    854
	  7 | 922445525 | 2019-08-02 |       1 |    963
	  8 | 156585478 | 2019-08-02 |       1 |    851
	  9 | 452265425 | 2019-08-02 |       1 |     15
	 10 | 122556525 | 2019-08-02 |       1 |    254


**12)** Crear un nuevo usuario.

	INSERT INTO users(first_name, email, last_name) VALUES ('Mauricio', 'mauro@dl.com', 'Estay');
	
	select * from users;

	 id | first_name |     email     | last_name 
	----+------------+---------------+-----------
	  1 | Carlos     | carlos@dl.com | Cabezas
	  2 | Laura      | laura@dl.com  | Prieto
	  3 | Mauricio   | mauro@dl.com  | Estay


**13)** Seleccionar la cantidad de llamados de cada uno de los usuarios (nombre de usuario y cantidad
de llamadas).

	SELECT users.first_name, count(calls.id) FROM users FULL JOIN calls ON users.id = calls.user_id GROUP BY first_name;

	 first_name | count 
	------------+-------
	 Carlos     |     4
	 Laura      |     6
	 Mauricio   |     0


**14)** Obtener el usuario con mayor número de llamadas

	SELECT users.first_name, count(calls.id) FROM users FULL JOIN calls ON users.id = calls.user_id GROUP BY first_name order by count(calls.id) desc limit 1;


**15)** Seleccionar los llamados del usuario llamado Carlos ordenados por fecha en orden descendente.

	SELECT users.first_name, calls.* FROM calls FULL JOIN users ON calls.user_id = users.id where calls.user_id = 1 GROUP BY users.first_name, calls.id order by calls.date desc; 

	 first_name | id |   phone   |    date    | user_id | length 
	------------+----+-----------+------------+---------+--------
	 Carlos     |  7 | 922445525 | 2019-08-02 |       1 |    963
	 Carlos     |  8 | 156585478 | 2019-08-02 |       1 |    851
	 Carlos     |  9 | 452265425 | 2019-08-02 |       1 |     15
	 Carlos     | 10 | 122556525 | 2019-08-02 |       1 |    254



**16)** Mostrar el nombre del usuario que haya hecho la llamada más larga

	SELECT users.first_name, MAX ( calls."length" ) AS duracion FROM calls INNER JOIN users ON calls.user_id = users.ID GROUP BY users.first_name ORDER BY duracion DESC LIMIT 1;

**17)** Mostrar todos los usuarios junto con la suma total de largo de llamadas

	SELECT users.first_name, SUM ( calls."length" ) AS Total FROM calls INNER JOIN users ON calls.user_id = users.ID GROUP BY users.first_name ORDER BY Total DESC;

**18)** Mostrar todos los usuarios que hayan hecho menos de 5 llamadas.

	SELECT users.first_name, count(calls.id) FROM users FULL JOIN calls ON users.id = calls.user_id GROUP BY first_name HAVING count(calls.id) < 5;

**19)** Nuevos cambios solicitados por cliente.Necesito agregar a la base una tabla de auditoría que registre el motivo del borrado de una
llamada y el usuario que lo efectuó.

    CREATE TABLE auditoria(id SERIAL Primary Key, motivo varchar(240), user_id integer, call_id integer);
    ALTER TABLE auditoria ADD FOREIGN KEY (user_id) REFERENCES users (id);
    ALTER TABLE auditoria ADD FOREIGN KEY (call_id) REFERENCES calls (id);

