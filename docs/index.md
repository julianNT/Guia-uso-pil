# Guías de simulación Processor-in-the-Loop en MATLAB/Simulink

Las guías de este sitio forman parte del trabajo de título "Implementación y caracterización de algoritmos de control en plataformas embebidas mediante simulaciones Processor-in-the-Loop en Matlab". El trabajo lo realiza Julián Núñez para optar al título de Ingeniero Civil Electrónico de la Universidad Técnica Federico Santa María.

Las guías muestran cómo verificar un algoritmo de control sobre hardware embebido mediante simulaciones Processor-in-the-Loop (PIL). En una simulación PIL, el computador de desarrollo compila el código generado para el procesador objetivo. Luego lo descarga y lo ejecuta en ese procesador ([SIL and PIL Simulations](https://www.mathworks.com/help/ecoder/ug/about-sil-and-pil-simulations.html)). Con esta técnica se comprueba que el código generado es numéricamente equivalente al modelo y se mide su tiempo de ejecución ([SIL and PIL Simulations](https://www.mathworks.com/help/ecoder/ug/about-sil-and-pil-simulations.html)).

Las guías siguen un orden progresivo, desde la instalación del entorno hasta un controlador PID ejecutándose en la placa.

- [Guía 1](Guia_1_Instalacion/Guia_1.md): Instalación del entorno.
- [Guía 2](Guia_2_PID_STM32/Guia_2.md): Controlador PID con PIL en STM32F767ZI.

Las guías para Raspberry Pi 5 y para un segundo controlador se agregarán en una etapa posterior.

Los modelos y scripts de cada guía se encuentran en el siguiente repositorio:

[COMPLETAR: URL del repositorio]

## Plataforma utilizada

Las guías se desarrollaron con las siguientes herramientas:

- MATLAB y Simulink [COMPLETAR: versión, por ejemplo R2025b].
- Embedded Coder y soporte de hardware para STM32 (detalle en la [Guía 1](Guia_1_Instalacion/Guia_1.md)).

Todas las guías utilizan la placa NUCLEO-F767ZI.
