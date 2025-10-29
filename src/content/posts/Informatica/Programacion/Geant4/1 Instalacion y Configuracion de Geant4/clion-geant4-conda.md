---
title: Configuración del IDE CLion para Geant4 y ROOT en Entorno Conda
published: 2025-10-10
tags: [Geant4, ROOT, Conda, CLion, CMake]
category: Root
draft: false
---

# Configuración del IDE CLion para Geant4 y ROOT en Entorno Conda

Este documento explica cómo integrar **Geant4** y **ROOT** dentro del entorno de desarrollo **CLion**, utilizando un entorno **Conda**.  
El propósito es disponer de una configuración moderna que permita autocompletado, depuración y ejecución directa de simulaciones, facilitando el trabajo académico y científico con herramientas de física computacional.

---

## Requisitos

* Ubuntu 20.04 o superior  
* CLion (versión 2023 o posterior)  
* Entorno Conda con Geant4 y ROOT instalados  
* Conexión a Internet  
* Proyecto de Geant4 (nuevo o de ejemplo)

---

## Paso 1: Crear o abrir un proyecto

1. Inicie CLion.  
2. Seleccione **New Project** para crear un nuevo proyecto o **Open** para abrir uno existente (por ejemplo, un ejemplo oficial de Geant4).  

![](./Adjuntos/Pasted%20image%2020251010113644.png)

Esto cargará la estructura base del proyecto y preparará los archivos para su configuración mediante CMake.

---

## Paso 2: Importar el proyecto con CMake

1. En la ventana de selección, elija **“Import Project from CMake”**.  

![](./Adjuntos/Pasted%20image%2020251010112117.png)

2. En la ventana emergente de configuración, ubique la sección **CMake options** y agregue las siguientes líneas, modificando la ruta con su nombre de usuario:

```bash
   -DGeant4_DIR=/home/usuario/anaconda3/envs/geant-root-env/lib/cmake/Geant4
   -DCLHEP_DIR=/home/usuario/anaconda3/envs/geant-root-env/lib/cmake/CLHEP
   -DCMAKE_PREFIX_PATH=/home/usuario/anaconda3/envs/geant-root-env
````

**Ejemplo:**

```bash
-DGeant4_DIR=/home/angel/anaconda3/envs/geant-root-env/lib/cmake/Geant4
-DCLHEP_DIR=/home/angel/anaconda3/envs/geant-root-env/lib/cmake/CLHEP
-DCMAKE_PREFIX_PATH=/home/angel/anaconda3/envs/geant-root-env
```

3. Espere a que CLion procese el proyecto y configure el entorno.
   Este proceso puede tardar algunos minutos, ya que el IDE indexa los archivos y detecta las dependencias de Geant4 y ROOT.

---

## Paso 3: Configurar las variables de entorno

Para que Geant4 acceda correctamente a sus bases de datos físicas (tablas de interacción, decaimientos radiactivos, secciones eficaces, etc.), es necesario definir las variables de entorno correspondientes.

1. Abra en CLion el menú:
   `Run → Edit Configurations → Environment variables`.
2. Pegue el siguiente bloque, ajustando las rutas con su nombre de usuario:

   ```bash
   G4ABLADATA=/home/angel/anaconda3/envs/geant-root-env/share/Geant4/data/ABLA3.3;
   G4EMLOWDATA=/home/angel/anaconda3/envs/geant-root-env/share/Geant4/data/EMLOW8.6.1;
   G4ENSDFSTATEDATA=/home/angel/anaconda3/envs/geant-root-env/share/Geant4/data/ENSDFSTATE3.0;
   G4LEDATA=/home/angel/anaconda3/envs/geant-root-env/share/Geant4/data/EMLOW8.6.1;
   G4NDL=/home/angel/anaconda3/envs/geant-root-env/share/Geant4/data/NDL4.7.1;
   G4PARTICLEXSDATA=/home/angel/anaconda3/envs/geant-root-env/share/Geant4/data/PARTICLEXS4.1;
   G4RadioactiveDecay=/home/angel/anaconda3/envs/geant-root-env/share/Geant4/data/RadioactiveDecay6.1.2
   ```

Una vez guardadas, las variables quedarán disponibles para todas las ejecuciones del proyecto dentro de CLion.

---

## Paso 4: Compilar el proyecto

Haga clic en el ícono del martillo en la barra superior para iniciar la compilación:

![](./Adjuntos/Pasted%20image%2020251010112651.png)

CLion ejecutará CMake, enlazará las bibliotecas de Geant4 y ROOT desde el entorno Conda, y generará los binarios necesarios.
Durante la primera compilación puede tardar algunos minutos mientras el IDE indexa y enlaza las dependencias.

---

## Paso 5: Ejecutar la simulación

Ejecute el proyecto con cualquiera de las siguientes opciones:

* **F9:** Ejecutar directamente (`Run`)
* **F10:** Ejecutar con depuración (`Debug`)

![](./Adjuntos/Pasted%20image%2020251010112815.png)

Si la configuración fue correcta, la simulación se ejecutará mostrando resultados en la consola o en una ventana gráfica, dependiendo del ejemplo utilizado.

---

## Paso 6: Visualizar o guardar la salida de terminal

CLion permite redirigir o guardar la salida de las simulaciones.
Para ello, haga clic en el ícono de impresión ubicado en el panel inferior:

![](./Adjuntos/Pasted%20image%2020251010112951.png)

Seleccione **“Save Output”** para exportar los resultados de la simulación a un archivo de texto.
Esto resulta útil para documentar ejecuciones o incluir datos en reportes técnicos.

---

## Notas
Aunque CLion permite ejecutar y compilar desde el propio IDE, es más recomendable usar CLion para escribir código y editarlo, y usar la terminal nativa del SO, para construir con `make`, `cmake` y ejecutar con `.programa`.

![alt text](./Adjuntos/Pasted%20image%2020251012171536-2.png)