---


---

<h2 id="desafío---postgresql">Desafío - PostgreSQL</h2>
<p>En este desafío trabajaremos con las diferentes queries desde el terminal de postgres.</p>
<p>Para desarrollar esta desafío, tendrán que anotar cada una de las queries que utilizaron en un archivo txt o md y subir este archivo a la plataforma.</p>
<p>Deben también ingresar, al final de este archivo, el nombre de los integrantes del grupo que participaron en el desarrollo. (Cada integrante debe subir el archivo).</p>
<p>Crear base de datos en base a los requerimientos enviados por el cliente.</p>
<p><strong>1)</strong> Crear una base de datos llamada call_list</p>
<pre><code>CREATE DATABASE call_list;
</code></pre>
<p><strong>2)</strong> En la base de datos recien creada, crear la tabla users con los campos id (clave primaria), first_name, email.</p>
<pre><code>        \c call_list
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
</code></pre>
<p><strong>3)</strong> Ingresar un usuario, llamado Carlos (el resto de los datos deben inventarlos).</p>
<pre><code>INSERT INTO users (first_name, email) VALUES ('Carlos', 'carlos@dl.com');
</code></pre>
<p><strong>4)</strong> Ingresar un usuario, llamada Laura (el resto de los datos deben inventarlos).</p>
<pre><code>INSERT INTO users (first_name, email) VALUES ('Laura', 'laura@dl.com');
</code></pre>
<p><strong>5)</strong> Crear una tabla llamada calls con los campos id (clave primaria), phone, date, user_id (foreign key relacionado a users) y length (para guardar la duración de la llamada) como un número entero.</p>
<pre><code>CREATE TABLE calls(id SERIAL PRIMARY KEY, 
			phone NUMERIC, 
			date DATE, 
			user_id INTEGER, 
			length INTEGER);

ALTER TABLE calls ADD FOREIGN KEY (user_id) REFERENCES users (id);
</code></pre>
<p><strong>6)</strong> Agregar a la tabla users el campo last_name.</p>
<pre><code>ALTER TABLE users ADD COLUMN last_name VARCHAR(30);
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
</code></pre>
<p><strong>7)</strong> Modelar la base de datos (agregar print de pantalla [utilizar <a href="https://www.draw.io/">https://www.draw.io/</a>]).</p>
<p><img src="https://drive.google.com/open?id=1lTA_3M6CfIQeuu2TaLuk96sCtphu6PP6" alt="Modelado de base de datos"></p>
<p><strong>8)</strong> Ingresar el apellido del usuario Carlos.</p>
<pre><code>UPDATE users SET last_name ='Cabezas' WHERE id=1;

select * from users;

 id | first_name |     email     | last_name 
----+------------+---------------+-----------
  2 | Laura      | laura@dl.com  | 
  1 | Carlos     | carlos@dl.com | Cabezas
</code></pre>
<p><strong>9)</strong> Ingresar el apellido del usuario Laura.</p>
<pre><code>UPDATE users SET last_name ='Prieto' WHERE id=2;

select * from users;

 id | first_name |     email     | last_name 
----+------------+---------------+-----------
  1 | Carlos     | carlos@dl.com | Cabezas
  2 | Laura      | laura@dl.com  | Prieto
</code></pre>
<p><strong>10)</strong> Ingresar 6 llamadas asociadas al usuario Laura.</p>
<pre><code>        INSERT INTO calls(phone, date, user_id, length) VALUES (985632145, NOW(), 2, 68);
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
</code></pre>
<p><strong>11)</strong> Ingresar 4 llamadas asociadas al usuario Carlos.</p>
<pre><code>INSERT INTO calls(phone, date, user_id, length) VALUES (922445525, NOW(), 1, 963);
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
</code></pre>
<p><strong>12)</strong> Crear un nuevo usuario.</p>
<pre><code>INSERT INTO users(first_name, email, last_name) VALUES ('Mauricio', 'mauro@dl.com', 'Estay');

select * from users;

 id | first_name |     email     | last_name 
----+------------+---------------+-----------
  1 | Carlos     | carlos@dl.com | Cabezas
  2 | Laura      | laura@dl.com  | Prieto
  3 | Mauricio   | mauro@dl.com  | Estay
</code></pre>
<p><strong>13)</strong> Seleccionar la cantidad de llamados de cada uno de los usuarios (nombre de usuario y cantidad<br>
de llamadas).</p>
<pre><code>SELECT users.first_name, count(calls.id) FROM users FULL JOIN calls ON users.id = calls.user_id GROUP BY first_name;

 first_name | count 
------------+-------
 Carlos     |     4
 Laura      |     6
 Mauricio   |     0
</code></pre>
<p><strong>14)</strong> Obtener el usuario con mayor número de llamadas</p>
<pre><code>SELECT users.first_name, count(calls.id) FROM users FULL JOIN calls ON users.id = calls.user_id GROUP BY first_name order by count(calls.id) desc limit 1;
</code></pre>
<p><strong>15)</strong> Seleccionar los llamados del usuario llamado Carlos ordenados por fecha en orden descendente.</p>
<pre><code>SELECT users.first_name, calls.* FROM calls FULL JOIN users ON calls.user_id = users.id where calls.user_id = 1 GROUP BY users.first_name, calls.id order by calls.date desc; 

 first_name | id |   phone   |    date    | user_id | length 
------------+----+-----------+------------+---------+--------
 Carlos     |  7 | 922445525 | 2019-08-02 |       1 |    963
 Carlos     |  8 | 156585478 | 2019-08-02 |       1 |    851
 Carlos     |  9 | 452265425 | 2019-08-02 |       1 |     15
 Carlos     | 10 | 122556525 | 2019-08-02 |       1 |    254
</code></pre>
<p><strong>16)</strong> Mostrar el nombre del usuario que haya hecho la llamada más larga</p>
<pre><code>SELECT users.first_name, MAX ( calls."length" ) AS duracion FROM calls INNER JOIN users ON calls.user_id = users.ID GROUP BY users.first_name ORDER BY duracion DESC LIMIT 1;
</code></pre>
<p><strong>17)</strong> Mostrar todos los usuarios junto con la suma total de largo de llamadas</p>
<pre><code>SELECT users.first_name, SUM ( calls."length" ) AS Total FROM calls INNER JOIN users ON calls.user_id = users.ID GROUP BY users.first_name ORDER BY Total DESC;
</code></pre>
<p><strong>18)</strong> Mostrar todos los usuarios que hayan hecho menos de 5 llamadas.</p>
<pre><code>SELECT users.first_name, count(calls.id) FROM users FULL JOIN calls ON users.id = calls.user_id GROUP BY first_name HAVING count(calls.id) &lt; 5;
</code></pre>
<p><strong>19)</strong> Nuevos cambios solicitados por cliente.Necesito agregar a la base una tabla de auditoría que registre el motivo del borrado de una<br>
llamada y el usuario que lo efectuó.</p>
<pre><code>CREATE TABLE auditoria(id SERIAL Primary Key, motivo varchar(240), user_id integer, call_id integer);
ALTER TABLE auditoria ADD FOREIGN KEY (user_id) REFERENCES users (id);
ALTER TABLE auditoria ADD FOREIGN KEY (call_id) REFERENCES calls (id);
</code></pre>

