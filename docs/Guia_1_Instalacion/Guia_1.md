# Guía 1: Instalación del entorno

## Objetivo

En esta guía el lector instala los productos de MathWorks y las herramientas de terceros necesarias para ejecutar simulaciones PIL sobre una placa NUCLEO-F767ZI. Al finalizar, el computador reconoce la placa y queda listo para la [Guía 2](../Guia_2_PID_STM32/Guia_2.md).

## Contexto

Una simulación PIL necesita tres elementos. El primero son productos de MathWorks que generan código C desde Simulink. El segundo es un paquete de soporte para la placa, y el tercero es una toolchain que compila el código para el procesador. La toolchain corresponde a un compilador cruzado, porque el código se ejecuta en una plataforma distinta del computador anfitrión ([Embedded Coder Requirements](https://www.mathworks.com/support/requirements/embedded-coder.html)).

Las secciones siguientes instalan cada elemento en ese mismo orden.

## Productos de MathWorks

La Tabla 1 lista los productos necesarios. Embedded Coder requiere MATLAB y MATLAB Coder, y necesita Simulink Coder para generar código desde Simulink ([Embedded Coder Requirements](https://www.mathworks.com/support/requirements/embedded-coder.html)). El modo PIL del bloque Model requiere una licencia de Embedded Coder ([Model block](https://www.mathworks.com/help/simulink/slref/model.html)).

*Tabla 1: Productos de MathWorks requeridos.*

| Producto                         | Uso en estas guías                                          |
| -------------------------------- | ----------------------------------------------------------- |
| MATLAB                           | Entorno base.                                               |
| Simulink                         | Modelado del controlador y de la planta.                    |
| MATLAB Coder                     | Prerrequisito de Embedded Coder.                            |
| Simulink Coder                   | Generación de código C desde Simulink.                      |
| Embedded Coder                   | Código para procesadores embebidos y simulaciones SIL/PIL.  |
| Control System Toolbox (opcional) | Discretización de la planta con `c2d` en la Guía 2.        |

Embedded Coder también requiere un compilador de C en el computador anfitrión ([Embedded Coder Requirements](https://www.mathworks.com/support/requirements/embedded-coder.html)). La lista de compiladores compatibles para Windows está en [Supported Compilers](https://www.mathworks.com/support/requirements/supported-compilers.html).

Para revisar los productos instalados, abra **Home > Add-Ons > Manage Add-Ons**. La ventana muestra la lista **Installed Add-Ons** ([Hardware Setup for STM32](https://www.mathworks.com/help/stm32b/gs/hardware-setup-stm32.html)).

## Soporte de hardware para STM32

MathWorks entrega el soporte para STM32 como un complemento de MATLAB, cuyo nombre depende de la versión:

- Hasta R2025b: Embedded Coder Support Package for STMicroelectronics STM32 Processors.
- Desde R2026a: STM32 Microcontroller Blockset, que reemplaza al paquete anterior.

Ambos nombres y el reemplazo aparecen en la [página del complemento en File Exchange](https://www.mathworks.com/matlabcentral/fileexchange/43093-embedded-coder-support-package-for-stmicroelectronics-stm32-processors). La misma página indica que el complemento permite ejecutar PIL con medición del tiempo de ejecución, y enlaza un video de instalación.

La familia STM32F7xx, a la que pertenece la NUCLEO-F767ZI, tiene soporte desde MATLAB R2022a ([Supported STM32 Processors](https://www.mathworks.com/help/stm32b/ug/supported-stm32-boards.html)). Instale el complemento que corresponda a su versión de MATLAB.

## Herramientas de terceros

El complemento necesita herramientas externas para configurar periféricos y compilar el código. Un asistente de configuración las instala y las registra en MATLAB ([Hardware Setup for STM32](https://www.mathworks.com/help/stm32b/gs/hardware-setup-stm32.html)). El asistente se abre de dos formas:

- Desde la ventana de comandos, con `stm32setup`.
- Desde **Home > Add-Ons > Manage Add-Ons**, presionando **Options** junto al complemento y luego **Setup**.

La Tabla 2 resume las herramientas relevantes para estas guías ([Hardware Setup for STM32](https://www.mathworks.com/help/stm32b/gs/hardware-setup-stm32.html)).

*Tabla 2: Herramientas de terceros instaladas por el asistente.*

| Herramienta                    | Función                                                  |
| ------------------------------ | -------------------------------------------------------- |
| STM32CubeMX                    | Genera el código C de inicialización de periféricos.     |
| GNU Tools                      | Compila la aplicación para el procesador Arm.            |
| STM32CubeProgrammer (opcional) | Descarga el ejecutable al microcontrolador.              |

MathWorks verifica versiones específicas de cada herramienta para cada versión de MATLAB. La Tabla 3 muestra las dos versiones más recientes ([Supported STM32 Processors](https://www.mathworks.com/help/stm32b/ug/supported-stm32-boards.html)). Si su versión es anterior, consulte la tabla completa en la misma fuente.

*Tabla 3: Versiones verificadas de herramientas de terceros.*

| MATLAB | STM32CubeMX | STM32CubeProgrammer | GNU Tools |
| ------ | ----------- | ------------------- | --------- |
| R2025b | 6.12.0      | 2.17.0              | 13.2.1    |
| R2026a | 6.12.0      | 2.17.0              | 13.2.1    |

!!! note "Pantalla del asistente"
    Si el asistente se ve distorsionado, ajuste el escalado de texto de Windows a 100 % ([Hardware Setup for STM32](https://www.mathworks.com/help/stm32b/gs/hardware-setup-stm32.html)).

## Conexión de la placa

La NUCLEO-F767ZI incluye un depurador ST-LINK/V2-1 integrado ([Zephyr: Nucleo F767ZI](https://docs.zephyrproject.org/latest/boards/st/nucleo_f767zi/doc/index.html)). El ST-LINK permite programar la placa y además expone un puerto COM virtual en el computador.

Conecte la placa con un cable Micro USB al conector del ST-LINK. Luego abra el Administrador de dispositivos de Windows y anote el número del puerto que aparece en **Puertos (COM y LPT)** ([Serial Configuration for PIL](https://www.mathworks.com/help/stm32b/ug/External-mode-PIL.html)). La Guía 2 utiliza ese número para configurar la comunicación PIL.

!!! info "Figura 1 (pendiente)"
    Captura del Administrador de dispositivos con el puerto COM del ST-LINK.

## Validación

Para validar la instalación, ejecute el tutorial oficial [Get Started with STMicroelectronics STM32 Processor Based Boards](https://www.mathworks.com/help/stm32b/ug/Getting-started-stm32cubemx.html). El tutorial compila un modelo de Simulink y lo ejecuta en la placa. Si el tutorial termina sin errores, el entorno funciona.

Con el entorno instalado, la [Guía 2](../Guia_2_PID_STM32/Guia_2.md) construye un controlador PID y lo ejecuta en la placa mediante PIL.
