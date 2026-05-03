# DevOps
Devops Information project
Curso de Unaj - centro profesional - Notas


1.Linux y DevOps
Como S.O.
El Kernel (Núcleo): Es el intermediario entre el hardware y el software. Gestiona la memoria, 
los procesos y el acceso al disco. 
El Shell (La interfaz): En servidores, no usamos mouse. El Shell (como Bash o Zsh) es el 
intérprete de comandos que nos permite hablar con el Kernel. 

2.Filesystem Hierarchy Standard (FHS)
es una norma técnica que define los nombres de los directorios y sus funciones dentro de un 
sistema operativo tipo Unix. Linux lo adopta principalmente para garantizar la 
interoperabilidad y la previsibilidad del sistema.
  #archivos de configuración siempre deben ir en /etc
  #los binarios ejecutables en /bin o /usr/bin 
independientemente de la "familia" de Linux que se utilice.

El FHS permite separar los archivos según su naturaleza: 
● Estáticos vs. Variables: Los archivos que no cambian (como el software en /usr) 
pueden montarse como "solo lectura" para mejorar la seguridad. 
● Compartibles vs. No compartibles: Permite que ciertos directorios se compartan a 
través de una red entre múltiples máquinas (como /opt o /var/mail) mientras que 
otros permanecen locales y únicos para cada host (como /etc). 

Es muy común poner /home en un disco aparte. Si necesitas reinstalar 
todo el sistema operativo (que vive en /), puedes formatear esa partición sin tocar tus 
documentos personales, ya que el FHS garantiza que los datos del usuario están aislados 
en su propio camino jerárquico.

3. Compatibilidad con el Ecosistema UNIX 
Linux heredó gran parte de su diseño de UNIX. Seguir el FHS asegura 
que las herramientas tradicionales y los scripts de administración creados hace décadas 
sigan funcionando hoy en día, manteniendo la coherencia histórica del ecosistema de 
software libre.

4.Esquema de particiones 
En la arquitectura de archivos de Debian, cada partición cumple una función estratégica 
para garantizar el aislamiento de datos y la estabilidad del sistema.
Raíz (/) 
Es el nivel superior de la jerarquía de archivos y el punto de partida de todo el sistema. 
Contiene los archivos esenciales para el arranque y las configuraciones críticas del kernel. 

¿Si se llena? 
● Consecuencia: El sistema puede dejar de arrancar o quedarse congelado en la 
pantalla de inicio de sesión (porque no puede escribir archivos temporales 
necesarios para la interfaz gráfica). 
● Síntoma: Servicios críticos fallan al intentar crear archivos de bloqueo (locks). No 
podrás instalar ni actualizar software. El usuario root suele tener un pequeño 
porcentaje de espacio reservado (5%) para poder entrar y borrar archivos, pero para 
un usuario normal, el sistema será inútil.

Swap 
No es un sistema de archivos tradicional, sino un espacio de intercambio que el sistema 
utiliza cuando la memoria RAM física está llena. Es vital para evitar que el sistema se 
"congele" ante picos de carga de trabajo.  el sistema 
mueve los datos que no estás usando en ese instante a la swap para liberar RAM real para 
el proceso activo. 

Partición de inicio (/boot) 
Es, posiblemente, la parte más crítica del sistema de archivos durante los primeros 
segundos de encendido de la computadora. Su función principal es almacenar todo lo 
necesario para que el sistema operativo pueda iniciar (arrancar). 
Dentro de este bloque, puedes ver tres componentes fundamentales: 
1. Kernel (vmlinuz): El corazón del sistema operativo.
2. 2. Initrd/Initramfs: El sistema de archivos temporal que carga controladores básicos. 
3. Bootloader (GRUB): El cargador de arranque que inicia el proceso. 
¿Si se llena? 
● Consecuencia: El sistema seguirá funcionando normalmente mientras esté 
encendido, pero no podrás actualizar el Kernel. 
● Síntoma: Al intentar un apt upgrade, recibirás un error de "No queda espacio en el 
dispositivo". Si intentas regenerar el menú de GRUB, fallará. El riesgo real ocurre en 
el próximo reinicio: si una actualización de kernel quedó a medias, el sistema podría 
no arrancar.
Archivos Temporales (/tmp) 
Este directorio se utiliza para almacenar archivos temporales creados por el sistema y las 
aplicaciones. En muchas configuraciones de Debian, su contenido se elimina 
automáticamente al reiniciar. 
Ejemplo: Cuando descargas una actualización o descomprimes un archivo comprimido, el 
instalador suele crear carpetas temporales aquí que se borran una vez finalizada la tarea. 
¿Si se llena? 
● Consecuencia: Muchas aplicaciones modernas (navegadores, editores de texto, 
herramientas de oficina) fallarán o se cerrarán inesperadamente. 
● Síntoma: No podrás descomprimir archivos grandes, las sesiones de usuario 
pueden cerrarse solas y muchos scripts de mantenimiento fallarán. Es un caos 
silencioso: el sistema parece "vivo", pero nada funciona correctamente.

Unix System Resources (/usr) 
usr no significa "User" (usuario) en el 
sentido de que ahí deban ir tus documentos personales (para eso existe /home). 
Contiene la mayoría de los programas, bibliotecas y documentación del sistema. Es, por 
definición, el lugar donde reside el software que no es crítico para el arranque inicial, pero sí 
para la operación del usuario. 
Ejemplo: Cuando instalas una herramienta como Python o el servidor web Apache, sus 
archivos ejecutables se almacenan generalmente en /usr/bin o /usr/sbin. 
¿Si se llena? 
● Consecuencia: No se puede instalar software nuevo ni actualizar el existente. 
● Síntoma: Dado que los binarios y librerías viven aquí, el sistema se vuelve 
"estático". No notarás errores en los programas que ya están corriendo, pero 
cualquier comando que intente crear un archivo en esta jerarquía (como un 
compilador o un gestor de paquetes) fallará inmediatamente.

Archivos “variables” (/var) 
Esta partición está destinada a datos que crecen o cambian frecuentemente durante el 
funcionamiento normal del equipo. Separar esta partición evita que un crecimiento 
desmedido de logs llene el disco principal. 
Ejemplo: Todos los mensajes de error del sistema se guardan en /var/log, y si manejas una 
base de datos, los datos reales de las tablas suelen vivir en /var/lib/mysql o directorios 
similares. 
¿Si se llena? 
● Consecuencia: Los servicios que escriben datos constantemente fallarán.
 Síntoma: Los servidores de bases de datos (como MySQL o PostgreSQL) se 
detendrán para evitar la corrupción de datos. Los correos electrónicos no entrarán (si 
usas /var/mail). Lo más común es que los logs dejen de registrarse, lo que te deja a 
ciegas ante cualquier otro error del sistema.
############################################################################################
El Rol de Linux en el Ciclo DevOps 
DevOps no es una herramienta, es una cultura de colaboración. Linux es el lenguaje común 
entre Desarrolladores y Operaciones. 
● Infraestructura como Código (IaC): Linux permite que configuremos servidores 
mediante scripts o herramientas como Ansible/Terraform. No se "instala a mano", se 
"programa la instalación". 
● Contenedores y Microservicios: Tecnologías como Docker y Kubernetes son 
nativas de Linux. Utilizan características del kernel (namespaces y cgroups) para 
aislar aplicaciones. 
● Estandarización: Un desarrollador escribe código en su laptop (posiblemente Linux 
o Mac) y ese código corre exactamente igual en un servidor Debian en la nube. 
● Escalabilidad: Puedes desplegar 1,000 instancias de Debian sin paagar licencias y 
con un consumo de RAM mínimo comparado con otros sistemas. 
¿cual es la metodología de “devOps”? 
Para entender esto, hay que dejar de verlo como un software que se instala y empezar a 
verlo como una metodología de trabajo (cultura) que busca eliminar la pared que 
históricamente dividía a los desarrolladores (Dev) de los administradores de sistemas (Ops). 
l Ciclo de Vida de DevOps símbolo de infinito porque es un proceso que nunca 
termina; cada entrega alimenta la siguiente mejora. 
1. Plan (Planificar): Definir qué se va a construir basándose en las necesidades del 
negocio. 
2. Code (Codificar): Los desarrolladores escriben el código y lo suben a un repositorio 
central (como Git). 
3. Build (Construir): Se compila el código y se preparan los artefactos (ej. una imagen 
de Docker). 
4. Test (Probar): Se ejecutan pruebas automatizadas para asegurar que el código no 
tenga errores. 
5. Release (Liberar): El código está listo para ser desplegado. 
6. Deploy (Desplegar): El software se instala en el servidor de producción (aquí es 
donde Linux es el rey).
7. Operate (Operar): Se mantiene el servicio funcionando, escalando recursos si es 
necesario. 
8. Monitor (Monitorear): Se analizan logs y métricas para detectar fallos o áreas de 
mejora, reiniciando el ciclo. .

<img width="371" height="208" alt="image" src="https://github.com/user-attachments/assets/23f4f8e2-2996-48dd-9820-fcee387fac0a" />

3. Los Pilares: El Modelo C.A.L.M.S. 
Para que la metodología funcione, se deben cumplir estos cinco principios: 
1. Culture (Cultura): Colaboración estrecha. Si algo falla en producción, es 
responsabilidad de todos, no solo de "el de sistemas". 
2. Automation (Automatización): Es la clave. Si una tarea se hace más de dos veces, 
debe automatizarse (scripts de Bash, CI/CD, despliegues automáticos). 
3. Lean (Bonito): Eliminar lo que no aporta valor. Hacer entregas pequeñas y 
frecuentes en lugar de una gigante cada seis meses. 
4. Measurement (Medición): No puedes mejorar lo que no mides. Se usan métricas 
de rendimiento, errores y tiempo de respuesta. 
5. Sharing (Compartir): Documentación abierta y comunicación constante. Lo que 
aprendió un equipo debe servirle al resto.

4. Conceptos Técnicos Clave en DevOps 
Para aplicar esta metodología en servidores Linux/Debian, debemos escuchar estos 
términos constantemente: 
● CI/CD (Integración y Despliegue Continuo):  
Automatizar el paso del código desde la PC del programador hasta el servidor final 
sin intervención manual. 
● IaC (Infraestructura como Código):  
Definir servidores mediante archivos de texto (ej. Ansible o Terraform). Esto permite 
que la infraestructura sea replicable y versionable. 
● Microservicios: 
En lugar de una aplicación gigante, se dividen en pequeñas piezas que corren de 
forma independiente (usualmente en contenedores).

En resumen: DevOps se trata de velocidad con seguridad. No sirve de nada entregar rápido 
si el servidor se cae, ni sirve que el servidor sea estable si tardamos meses en sacar una 
actualización.

Conceptos Clave de Operación (CLI) 
cualquier flujo de trabajo de DevOps exitoso, permitiendo pasar de la gestión 
manual a la automatización a gran escala. Con sus tres pilares que funcionan como un 
conjunto de herramientas cohesivo, otorga al administrador la visibilidad necesaria para 
monitorear el estado del sistema, la agilidad para transformar configuraciones mediante 
código y la capacidad analítica para resolver problemas de conectividad en entornos 
distribuidos
Estos son los pilares: 
● Gestión de Procesos: Cómo ver qué está consumiendo recursos (top, htop) y cómo 
detener servicios que fallan (systemctl). 
● Manipulación de Texto: En DevOps, configurar es editar texto. Herramientas como 
grep (buscar), sed (reemplazar) y awk (procesar datos) son indispensables para 
automatizar. 
● Redes Básicas: Entender cómo Linux ve la red (ip addr), cómo prueba conectividad 
(ping, traceroute) y cómo escucha puertos (netstat o ss). 

Filosofía Open Source y Seguridad 

Es un modelo de desarrollo y licenciamiento basado en cuatro libertades 
fundamentales: 
1. Libertad de ejecución: Usar el programa para cualquier propósito. 
2. Libertad de estudio: Estudiar cómo funciona el programa y adaptarlo (acceso al 
código fuente). 
3. Libertad de redistribución: Copiar y distribuir el software para ayudar a otros. 
4. Libertad de mejora: Mejorar el programa y publicar las mejoras para que toda la 
comunidad se beneficie. 
Esta filosofía fomenta la colaboración masiva, la transparencia y la meritocracia técnica.

El Vínculo con la Seguridad: Transparencia vs. Oscuridad 
La relación entre Open Source y Seguridad se basa en un debate histórico: 
● Seguridad por Oscuridad (Modelo Propietario) 
○ Concepto: Se basa en la premisa de que si un atacante no puede ver el 
código fuente, no puede encontrar sus vulnerabilidades. 
○ La Realidad: Los atacantes avanzados no necesitan el código fuente; 
pueden encontrar agujeros mediante ingeniería inversa o analizando el 
comportamiento del software en ejecución (fuzzing). Cuando se descubre un 
fallo, los usuarios dependen enteramente de que la empresa propietaria cree 
y distribuya un parche, lo cual puede llevar tiempo. 
● Seguridad por Transparencia (Modelo Open Source) 
○ Concepto: Se basa en la Ley de Linus (formulada por Eric S. Raymond): 
"Dadas suficientes miradas, todos los errores son superficiales". 
○ La Realidad: Al exponer el código fuente a todo el mundo, las 
vulnerabilidades pueden ser descubiertas y parcheadas por la comunidad 
mucho antes de que puedan ser explotadas masivamente. No dependes de 
un solo proveedor; la responsabilidad y la capacidad de corrección están 
distribuidas.


¿Por qué utilizar Linux como Sistema Operativo? 
La adopción de Linux en grandes organizaciones no es solo una cuestión de costos, sino de 
soberanía tecnológica, estabilidad y seguridad.
