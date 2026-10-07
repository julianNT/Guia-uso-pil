# Guía 1: Instalación del entorno
 
## Objetivo
 
Esta etapa deja instaladas las herramientas necesarias para usar la simulación PIL en las dos plataformas de este trabajo: la **STM32-NUCLEO-F767ZI** y la **Raspberry Pi 5**. Primero se instalan las herramientas comunes entre ambas, y luego lo específico de cada placa. Al finalizar esta guía se busca que el computador reconozca las dos placas y quede listo para las guías siguientes.
 
![Raspberry Pi 5](img/Raspberry-Pi-5.jpg){ width: 200px; }

![STM32-NUCLEO-F767ZI](img/Stm32f767zi.jfif){ width: 200px; }


## Contexto
 
Primero que nada, una simulación PIL necesita cinco elementos a descargar: (1) los productos de MathWorks que generan el código C desde Simulink, (2) un compilador de C en el computador, (3) un paquete de soporte para la placa, (4) una toolchain que compila ese código para el procesador de la placa ([Embedded Coder Requirements](https://www.mathworks.com/support/requirements/embedded-coder.html)) y (5) las herramientas propias de cada placa: STM32CubeMX en la STM32 y el sistema operativo en la Raspberry Pi. Los dos primeros son comunes a cualquier placa; los tres últimos dependen de cuál se use.
 
## Instalación común
 
### 1) Productos de MathWorks
A continuación se instalarán las herramientas proporcionadas por el propio MathWorks.
La Tabla 1 lista los productos necesarios para ambas plataformas.
 
*Tabla 1: Productos de MathWorks requeridos.*
 
| Producto        | Para qué sirve                                       |
| ---------------- | --------------------------------------------------  |
| MATLAB           | Entorno base.                                       |
| Simulink         | Entorno para simular y modelar.                     |
| MATLAB Coder     | Genera código C y C++ a partir de código MATLAB.    |
| Simulink Coder   | Genera el código C desde bloques en Simulink.       |
| Embedded Coder   | Genera código para sistemas embebidos y habilita simulaciones SIL/PIL.               |
| Control System Toolbox (opcional) | Modelar, analizar y sintonizar sistemas de control.|
| Compilador de C  | Necesario para compilar código C a código máquina.  |
 
Se instalan desde el Add-On Explorer de MATLAB. Para revisar lo ya instalado, se verifica en **Home > Add-Ons > Manage Add-Ons**.
 
### 2) Compilador de C
 
Embedded Coder también requiere un compilador de C en el computador ([Embedded Coder Requirements](https://www.mathworks.com/support/requirements/embedded-coder.html)). La lista de compiladores compatibles para Windows está en [Supported Compilers](https://www.mathworks.com/support/requirements/supported-compilers.html).
 
Por conveniencia, se utilizará el compilador MinGW para estas guías, debido a lo conocido y confiable que es ([link MinGW](https://la.mathworks.com/matlabcentral/fileexchange/52848-matlab-support-for-mingw-w64-c-c-fortran-compiler)).
 
## Instalación específica: STM32F767ZI
 
### 3) Paquete de soporte para STM32
 
El soporte para STM32 se instala como un complemento aparte, cuyo nombre depende de la versión de MATLAB:
 
- Hasta R2025b: Embedded Coder Support Package for STMicroelectronics STM32 Processors.
La familia STM32F7xx, a la que pertenece la NUCLEO-F767ZI, tiene soporte, por lo que su configuración es más fácil. (Si la versión de la NUCLEO no tiene soporte, se puede realizar la simulación PIL, pero antes hay que configurar los pines de comunicación en STM32CubeMX.) ([Embedded Coder Support Package for STM32 Processors](https://www.mathworks.com/matlabcentral/fileexchange/43093))
 
### 4) Toolchain para STM32
 
La toolchain de la STM32 es **GNU Tools**, que compila el código generado para el procesador Arm de la placa.
 
Se configura desde la ventana *Hardware Setup* del paquete de soporte, que se abre con el botón **Setup** del Add-On Manager ([Install Support for STMicroelectronics STM32 Processors](https://mathworks.com/help/ecoder/stmicroelectronicsstm32f4discovery/ug/install-support-for-stm32-board-processors.html)).
 
### 5) Herramientas propias de la STM32
 
La herramienta propia de esta placa es **STM32CubeMX**, que genera el código de inicialización de los periféricos. Es obligatoria para PIL, porque ahí se configura el puerto serial por el que el modelo se comunica con la placa ([Serial Configuration for Monitor & Tune and PIL](https://www.mathworks.com/help/ecoder/stmicroelectronicsstm32f4discovery/ug/External-mode-PIL.html)). La Guía 2 cubre esa configuración.
 
Opcionalmente, **STM32CubeProgrammer** descarga el ejecutable a la placa. La misma ventana *Hardware Setup* configura ambas herramientas.
 
Conecte la placa con un cable Micro USB al ST-LINK integrado. Luego, en el Administrador de dispositivos de Windows, anote el puerto COM que aparece en **Puerto COM** ([Serial Configuration for Monitor & Tune and PIL](https://www.mathworks.com/help/ecoder/stmicroelectronicsstm32f4discovery/ug/External-mode-PIL.html)).
 
 
 
## Instalación específica: Raspberry Pi 5
 
### 3) Paquete de soporte para Raspberry Pi 5
 
El soporte para Raspberry Pi se instala como otro complemento aparte, también con nombre distinto según la versión:
 
- Hasta R2025b: Simulink Support Package for Raspberry Pi Hardware.
Antes de instalarlo, la placa debe tener instalado su sistema operativo, que corresponde al elemento 5 y se describe más abajo.
 
Al instalar el complemento, un asistente de hardware guía la conexión con la placa e instala en ella las librerías necesarias para trabajar con MATLAB y Simulink ([Simulink Support Package for Raspberry Pi Hardware](https://www.mathworks.com/matlabcentral/fileexchange/40313-simulink-support-package-for-raspberry-pi-hardware)). Se abre desde **Manage Add-Ons > Options > Setup**.
 
A diferencia de STM32, la conexión con la Raspberry Pi es por red (TCP/IP), no por puerto serial: el asistente pide la dirección IP o el nombre de host de la placa.
 
### 4) Toolchain para Raspberry Pi 5
 
La toolchain que usa PIL con la Raspberry Pi es **GNU GCC Embedded Linux** ([SIL and PIL Verification for Reinforcement Learning](https://www.mathworks.com/help/reinforcement-learning/ug/sil-and-pil-verification-for-reinforcement-learning.html)). A diferencia de la STM32, no se instala en el computador: el asistente de hardware deja en la placa las herramientas necesarias, y en el modelo solo se comprueba que esté seleccionada. La Guía 2 cubre esa comprobación.
 
### 5) Herramientas propias de la Raspberry Pi 5
 
La herramienta propia de esta placa es su **sistema operativo**, que debe estar escrito en la tarjeta SD con Raspberry Pi Imager ([Instalador Pi Imager](https://www.raspberrypi.com/software/)).
 
Este es el único elemento que se instala antes que el paquete de soporte, porque el asistente de hardware necesita conectarse a una placa que ya esté funcionando ([Install Support for Raspberry Pi Hardware](https://www.mathworks.com/help/matlab/supportpkg/install-support-for-raspberry-pi-hardware.html)).
 
 
## Validación
 
Si ambos asistentes terminaron sin errores, el entorno está listo. La [Guía 2](../Guia_2_PID_STM32/Guia_2.md) construye un controlador PID y lo ejecuta en la NUCLEO-F767ZI mediante PIL.