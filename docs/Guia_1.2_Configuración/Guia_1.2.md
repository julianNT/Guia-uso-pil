# Guía 2: Configuración de las plataformas para PIL

## Objetivo

Esta etapa deja la NUCLEO-F767ZI y la Raspberry Pi 5 configuradas para ejecutar PIL desde Simulink. La configuración se hace una sola vez por modelo e indica en qué placa corre el código y por dónde se comunica con el computador. Al finalizar, el modelo queda listo para las guías siguientes, que solo agregan el subsistema a verificar.

## Contexto

Toda la configuración está en el cuadro **Configuration Parameters** del modelo (**Ctrl+E**), sección **Hardware Implementation**. Lo que cambia entre las dos plataformas es el canal de comunicación: la NUCLEO usa un puerto serial y la Raspberry Pi usa la red local, por lo que se configuran por separado.

## Configuración: NUCLEO-F767ZI

El periférico serial que usa PIL no se configura en Simulink, sino en un proyecto de STM32CubeMX que el modelo importa y desde el cual Simulink genera el código de inicialización ([Serial Configuration for Monitor & Tune and PIL](https://www.mathworks.com/help/ecoder/stmicroelectronicsstm32f4discovery/ug/External-mode-PIL.html)).

### Paso 1: Seleccionar la placa

Abra el modelo, presione **Ctrl+E** y despliegue **Hardware Implementation > Hardware board**.

Si la placa aparece por nombre, selecciónela; es el caso de la NUCLEO-F767ZI. Si no aparece, elija la opción custom de la serie de su microcontrolador: sirve para placas de diseño propio cuyo procesador pertenece a una serie soportada ([Configure STM32CubeMX with Simulink](https://www.mathworks.com/help/stm32b/ug/stm32-cubemx-configuration.html)). Cuál de los dos casos sea determina el trabajo del Paso 3.

> **[Agregar imagen: lista Hardware board desplegada, con la entrada de la NUCLEO-F767ZI y la opción custom de la serie F7xx.]**

### Paso 2: Crear el proyecto de STM32CubeMX

En **Build options**, use **Browse** si ya tiene un proyecto `.ioc`, o **Create** para crear uno nuevo: escriba el nombre con extensión `.ioc`, elija la carpeta, seleccione el hardware y confirme con **Apply** y **OK**. Conviene guardarlo junto al modelo, porque ambos archivos quedan amarrados. Luego presione **Launch** para abrirlo en STM32CubeMX ([Configure STM32CubeMX with Simulink](https://www.mathworks.com/help/stm32b/ug/stm32-cubemx-configuration.html)).

> **[Agregar imagen: sección Build options con los botones Browse, Create y Launch.]**

### Paso 3: Configurar el USART

**Placa con soporte.** El proyecto se creó desde la definición de la placa, así que los periféricos y sus pines ya vienen asignados con la configuración por defecto ([UM1718, manual de STM32CubeMX](https://www.st.com/resource/en/user_manual/um1718-stm32cubemx.pdf)). No hay que asignar pines: solo habilitar el periférico. En la NUCLEO-F767ZI es el USART3, porque el ST-LINK expone un puerto COM virtual hacia el computador conectado internamente a ese periférico, en los pines PD8 y PD9 ([UM1974, manual de las placas Nucleo-144](https://www.st.com/resource/en/user_manual/dm00244518.pdf)).

**Placa sin soporte.** El proyecto se creó solo desde el microcontrolador y parte sin pines asignados, así que antes hay que resolver dos cosas. En **Clock Configuration**, habilitar el cristal externo en el periférico **RCC** y ajustar el árbol de reloj a la frecuencia real de la placa; si el reloj queda mal, el baud rate efectivo no coincide con el nominal y la comunicación falla. En **Pinout & Configuration**, asignar los pines TX y RX del USART elegido, haciendo clic sobre ellos en el diagrama del encapsulado, según el esquemático de la placa.

> **[Agregar imagen: vista Pinout & Configuration con los pines TX y RX asignados, para el caso sin soporte.]**

En ambos casos, la configuración del periférico es la de la Tabla 1. Al terminar, guarde el proyecto ([Serial Configuration for Monitor & Tune and PIL](https://www.mathworks.com/help/ecoder/stmicroelectronicsstm32f4discovery/ug/External-mode-PIL.html)).

*Tabla 1: Configuración del USART en STM32CubeMX.*

| Parámetro | Valor |
| --- | --- |
| Mode | Asynchronous. |
| Baud Rate | El que usará el modelo; anótelo. |
| DMA Settings | Solicitud DMA para la recepción del USART. |

> **[Agregar imagen: configuración del USART3 en STM32CubeMX, con Mode, Baud Rate y la solicitud DMA.]**

### Paso 4: Indicar el puerto serial en el modelo

Conecte la placa con un cable Micro USB y anote el puerto COM que le asigna Windows, visible en el Administrador de dispositivos bajo **Puertos (COM y LPT)**.

Vuelva a **Configuration Parameters**, entre a **Hardware Implementation > Target hardware resources > Connectivity** e indique el **USART** del Paso 3 y ese **COM Port** ([Code Verification and Validation with PIL for STM32](https://www.mathworks.com/help/stm32b/ug/STM32F4xx-PIL-example.html)). El baud rate debe coincidir con el del proyecto de STM32CubeMX. Estos tres valores son la causa más común de que PIL no logre comunicarse.

> **[Agregar imagen: pestaña Connectivity con los campos USART y COM Port.]**

## Configuración: Raspberry Pi 5

Aquí no se configura hardware. Simulink se conecta a la placa por red como a un computador remoto, copia el código generado, lo compila en su Linux y ejecuta ahí el resultado, por lo que solo necesita una dirección y credenciales de acceso ([Model Configuration Simulink Support Package for Raspberry Pi Hardware](https://www.mathworks.com/help/simulink/supportpkg/raspberrypi_ref/raspberrypi-model-configuration-parameters.html)). La placa debe estar encendida y en la misma red cada vez que se ejecute PIL.

### Paso 1: Ejecutar el asistente de hardware

El asistente instala en la placa las librerías que necesita para trabajar con Simulink y deja guardadas la dirección y las credenciales; se pasa una vez por placa. Ábralo desde el **Add-On Manager**, en **Setup** del paquete de soporte, y siga las pantallas en orden: en *Review Required Packages and Libraries* confirme la instalación de los paquetes que lista ([Install Support for Raspberry Pi Hardware](https://www.mathworks.com/help/simulink/supportpkg/raspberrypi_ug/install-target-for-raspberry-pi-hardware.html)).

> **[Agregar imágenes: pantallas del asistente en orden, una por paso.]**

### Paso 2: Apuntar el modelo a la placa

Presione **Ctrl+E** y en **Hardware Implementation > Hardware board** seleccione **Raspberry Pi**. Eso carga los parámetros de la placa, de los cuales hay que completar los cuatro de la Tabla 2 ([Model Configuration Simulink Support Package for Raspberry Pi Hardware](https://www.mathworks.com/help/simulink/supportpkg/raspberrypi_ref/raspberrypi-model-configuration-parameters.html)).

*Tabla 2: Parámetros de conexión con la Raspberry Pi.*

| Parámetro | Valor |
| --- | --- |
| Device Address | Dirección IP o nombre de host de la placa. |
| Username | Usuario de Linux de la placa. |
| Password | Contraseña de ese usuario. |
| Build directory | Carpeta de compilación, dentro del Linux de la placa. |

La dirección IP se obtiene ejecutando `hostname -I` en la terminal de la placa, entre otros métodos ([Get IP Address of Raspberry Pi Hardware](https://www.mathworks.com/help/simulink/setup-and-configuration-raspberrypi.html)). Conviene fijarla en el router: si cambia, el modelo deja de encontrar la placa. El **Build directory** es una ruta de Linux, no de Windows.

Para confirmar lo registrado, ejecute `raspberrypi` en la ventana de comandos: devuelve el nombre de host, el usuario, la contraseña y el directorio de compilación almacenados ([Installation Setup and Configuration](https://www.mathworks.com/help/simulink/setup-and-configuration-raspberrypi.html)).

> **[Agregar imagen: panel de Hardware Implementation con los cuatro campos completados.]**

### Paso 3: Revisar la arquitectura y la toolchain

**Device type** indica para qué arquitectura se compila y debe corresponder al sistema operativo instalado en la placa: ARM Cortex-A (32-bit) o ARM Cortex-A (64-bit) ([Optimize Code for Raspberry Pi Using Code Replacement Library](https://www.mathworks.com/help/simulink/supportpkg/raspberrypi_ug/optimize-code-for-raspberry-pi-using-code-replacement-library.html)). Que la Raspberry Pi 5 soporte 64 bits no garantiza que la imagen instalada lo sea, así que verifíquelo en la placa.

En **Code Generation > Toolchain** debe estar *GNU GCC Embedded Linux*, el compilador cruzado que usa el ejemplo oficial de PIL con Raspberry Pi ([SIL and PIL Verification for Reinforcement Learning](https://www.mathworks.com/help/reinforcement-learning/ug/sil-and-pil-verification-for-reinforcement-learning.html)). Normalmente queda asignado al seleccionar la placa.

> **[Agregar imagen: campo Device type y panel Code Generation > Toolchain.]**

## Validación

Si el modelo tiene seleccionada la placa y los parámetros de comunicación completos, la configuración está lista. Guarde el modelo: la [Guía 3](../Guia_3_PID_STM32/Guia_3.md) construye un controlador PID y lo ejecuta en la NUCLEO-F767ZI mediante PIL.
