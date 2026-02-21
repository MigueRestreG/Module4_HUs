1. -- Creación de la base de datos principal
CREATE DATABASE sistema_gestion_universitaria;

-- Seleccionar la base de datos
USE sistema_gestion_universitaria;

-- Tabla que almacena la información de los estudiantes
CREATE TABLE estudiantes (
    id_estudiante INT PRIMARY KEY AUTO_INCREMENT,
    nombre_completo VARCHAR(100) NOT NULL,
    correo_electronico VARCHAR(150) UNIQUE,
    genero ENUM('Masculino','Femenino','Otro') NOT NULL,
    identificacion VARCHAR(50) NOT NULL UNIQUE,
    carrera VARCHAR(100) NOT NULL,
    fecha_nacimiento DATE NOT NULL,
    fecha_ingreso DATE NOT NULL
);

-- Tabla que almacena la información de los docentes
CREATE TABLE docentes (
    id_docente INT PRIMARY KEY AUTO_INCREMENT,
    nombre_completo VARCHAR(100) NOT NULL,
    correo_institucional VARCHAR(100) NOT NULL UNIQUE,
    departamento VARCHAR(100) NOT NULL,
    anios_experiencia INT NOT NULL
);

-- Tabla que contiene los cursos ofertados
CREATE TABLE cursos (
    id_curso INT PRIMARY KEY AUTO_INCREMENT,
    nombre_curso VARCHAR(50) NOT NULL,
    codigo VARCHAR(50) NOT NULL UNIQUE,
    creditos INT NOT NULL,
    semestre INT NOT NULL,
    id_docente INT,
    FOREIGN KEY (id_docente) REFERENCES docentes(id_docente)
    ON DELETE CASCADE
);

-- Tabla que relaciona estudiantes con cursos (matrículas)
CREATE TABLE matriculas (
    id_matricula INT PRIMARY KEY AUTO_INCREMENT,
    id_estudiante INT NOT NULL,
    id_curso INT NOT NULL,
    fecha_matricula DATE NOT NULL,
    nota_final DECIMAL(3,2) NOT NULL,
    FOREIGN KEY (id_estudiante) REFERENCES estudiantes(id_estudiante)
        ON DELETE CASCADE
        ON UPDATE CASCADE,
    FOREIGN KEY (id_curso) REFERENCES cursos(id_curso)
        ON DELETE CASCADE
        ON UPDATE CASCADE,
    UNIQUE (id_estudiante, id_curso)
);

2. Inserción de datos (Registros de ejemplo)

INSERT INTO estudiantes 
(nombre_completo, correo_electronico, genero, identificacion, carrera, fecha_nacimiento, fecha_ingreso)
VALUES
('Daniel Ríos', 'daniel.rios@mail.com', 'Masculino', '2001', 'Ingeniería de Sistemas', '2001-04-10', '2023-01-15'),
('Valentina Mejía', 'valentina.mejia@mail.com', 'Femenino', '2002', 'Psicología', '2000-09-18', '2022-08-20'),
('Sebastián Ochoa', 'sebastian.ochoa@mail.com', 'Masculino', '2003', 'Administración', '2002-02-25', '2023-02-01'),
('Camila Herrera', 'camila.herrera@mail.com', 'Femenino', '2004', 'Contaduría', '2001-11-05', '2022-07-10'),
('Andrés Salazar', 'andres.salazar@mail.com', 'Masculino', '2005', 'Derecho', '1999-06-30', '2021-01-12');

3. Registro de docentes

INSERT INTO docentes 
(nombre_completo, correo_institucional, departamento, anios_experiencia)
VALUES
('Laura Martínez', 'laura.martinez@uni.edu', 'Ingeniería', 9),
('Ricardo Gómez', 'ricardo.gomez@uni.edu', 'Ciencias Básicas', 15),
('Patricia Vélez', 'patricia.velez@uni.edu', 'Humanidades', 11);

4. Registro de cursos

INSERT INTO cursos 
(nombre_curso, codigo, creditos, semestre, id_docente)
VALUES
('Programación I', 'SIS101', 4, 1, 1),
('Estadística General', 'EST201', 3, 2, 2),
('Ética Profesional', 'HUM110', 2, 1, 3),
('Base de Datos I', 'SIS202', 4, 2, 1);

5. Registro de matrículas

INSERT INTO matriculas 
(id_estudiante, id_curso, fecha_matricula, nota_final)
VALUES
(1, 1, '2023-01-20', 4.3),
(1, 4, '2023-01-22', 4.7),
(2, 2, '2022-08-25', 3.9),
(3, 1, '2023-02-05', 4.1),
(3, 2, '2023-02-06', 3.8),
(4, 3, '2022-07-15', 4.5),
(5, 4, '2021-01-20', 4.0);


6. Gestión de usuarios, roles y permisos

Creación de rol académico
CREATE ROLE IF NOT EXISTS auditor_academico;

Asignación de permisos al rol
GRANT SELECT ON vista_historial_academico TO auditor_academico;

Creación de nuevo usuario
CREATE USER 'maria'@'localhost' IDENTIFIED BY 'claveSegura2026';

Asignación del rol al usuario
GRANT auditor_academico TO 'maria'@'localhost';

Activación del rol al iniciar sesión
SET ROLE auditor_academico;