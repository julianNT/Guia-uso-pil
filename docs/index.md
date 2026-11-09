# Guías de simulación Processor-in-the-Loop en MATLAB/Simulink

Las guías de este sitio forman parte del trabajo de título "Implementación y caracterización de algoritmos de control en plataformas embebidas mediante simulaciones Processor-in-the-Loop en Matlab". El trabajo lo realiza Julián Núñez para optar al título de Ingeniero Civil Electrónico de la Universidad Técnica Federico Santa María.

Este sitio busca facilitar la configuración de simulaciones Processor-in-the-Loop (PIL) en MATLAB/Simulink, mediante guías paso a paso dirigidas a quien no tiene experiencia previa con la herramienta. Por el momento, el sitio revisa las instalaciones necesarias para el hardware utilizado y muestra un ejemplo de controlador embebido verificado mediante PIL.

Las guías siguen un orden progresivo, desde la instalación del entorno hasta un controlador predictivo (MPC) ejecutándose en la placa.

- [Guía 1](Guia_1_Instalacion/Guia_1.md): Instalación del entorno.
- [Guía 2](Guia_2_Configuracion/Guia_2.md): Configuración de las plataformas para PIL.
- [Guía 3](Guia_3_PID_AntiWindup/Guia_3.md): Controlador PID con anti-windup con PIL en STM32 Nucleo F767ZI.
- [Guía 4](Guia_4_MPC/Guia_4.md): Controlador predictivo (MPC) con PIL en STM32 Nucleo F767ZI.


Los modelos y scripts de cada guía se encuentran en el siguiente repositorio:

[\[URL del repositorio\]](https://github.com/julianNT/Ejemplos-uso-de-PIL)

## Plataforma utilizada

Las guías se desarrollaron con las siguientes herramientas:

- MATLAB y Simulink R2025b.
- Embedded Coder y soporte de hardware para STM32 y Raspberry pi 5.

