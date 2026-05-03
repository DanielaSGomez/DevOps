¿Qué es una Distribución Linux? 
Aunque solemos referirnos a "Linux" como un sistema operativo, técnicamente es solo el 
kernel (núcleo). Una distribución (o distro) es el conjunto de software que rodea al kernel 
para convertirlo en un sistema operativo completo y funcional.

Una distribución típica incluye: 
● Kernel Linux: El gestor de hardware. 
● Gestor de paquetes: Herramientas para instalar, actualizar y eliminar software (ej. 
APT, DNF). 
● Entorno de escritorio: La interfaz visual (ej. GNOME, KDE, XFCE). 
● Repositorios: Servidores oficiales donde se aloja el software verificado

Clasificación de las Distribuciones 
El universo Linux se organiza principalmente en "familias" basadas en su distribución de 
origen y el formato de sus paquetes: 
● Familia Debian: Utiliza paquetes .deb. Es la más extendida y sirve de base para 
Ubuntu, Linux Mint, Kali Linux y la que vamos a tener como plataforma en este 
curso. 
● Familia Red Hat: Utiliza paquetes .rpm. Enfocada al sector empresarial (RHEL) y 
comunitario (Fedora, AlmaLinux, Rocky Linux). 
● Familia Arch: Enfocada en usuarios avanzados que buscan personalización total y 
software a la última versión (Rolling Release).

Debian: "El Sistema Operativo Universal"
Para garantizar la estabilidad, Debian organiza su desarrollo en tres ramas: 
● Stable (Estable): La joya de la corona. Es el software que ha pasado años de 
pruebas. Es la opción predilecta para servidores críticos donde no se puede permitir 
un error. 
● Testing (Pruebas): Contiene software más actual que está en proceso de 
estabilización para convertirse en la próxima versión estable. 
● Unstable (Inestable - "Sid"): Donde llega primero todo el software nuevo. Es el 
campo de experimentación. 

El sistema de gestión de paquetes: APT 
Debian revolucionó la administración de sistemas con APT (Advanced Package Tool). Antes 
de APT, instalar software en Linux era una pesadilla de dependencias manuales. APT 
automatizó este proceso, permitiendo que un simple comando (apt install) resuelva y 
descargue todo lo necesario para que un programa funcione. 

Por qué Debian es el estándar en Servidores y DevOps 
En el mundo profesional, Debian es valorado por: 
● Estabilidad Legendaria: Un servidor Debian puede estar encendido durante años 
sin necesidad de reiniciarse por fallos de software. 
● Seguridad: El equipo de seguridad de Debian es extremadamente rápido para 
parchear vulnerabilidades (CVE). 
● Ligereza: Una instalación base de Debian consume mínimos recursos, dejando toda 
la potencia del hardware disponible para la aplicación (bases de datos, servidores 
web, contenedores Docker). 
● Independencia: Al no ser propiedad de una empresa (como Red Hat de IBM o 
Ubuntu de Canonical), Debian no está sujeta a cambios de licencia repentinos o 
decisiones comerciales. 
