---
title: Instalacion Root y Geant4 en Entorno Conda
published: 2025-10-04
tags: [Markdown, Blogging, Demo]
category: Root
draft: false
---

# Instalacion Root y Geant4 en Entorno Conda

## Requisitos

* Ubuntu 20.04 o superior
* Conexión a Internet
* Terminal (Ctrl + Alt + T)

---

## Paso 1: Instalar Anaconda en Ubuntu

### 1.1 Descargar Anaconda (versión estable)

```bash
cd ~/Downloads
wget https://repo.anaconda.com/archive/Anaconda3-2024.02-1-Linux-x86_64.sh
```

### 1.2 Ejecutar el instalador

```bash
bash Anaconda3-2024.02-1-Linux-x86_64.sh
```

Presiona `Enter` para aceptar la licencia, y responde `yes` cuando pregunte si quieres modificar `.bashrc`.

### 1.3 Activar conda

Después de la instalación:

```bash
source ~/.bashrc
```

Verifica:

```bash
conda --version
```

---

## Paso 2: Crear un entorno para ROOT y Geant4

### 2.1 Establecer prioridad de canales

```bash
conda config --set channel_priority strict
```

### 2.2 Crear el entorno

```bash
conda create -n geant-root-env -c conda-forge root
```

### 2.3 Activarlo

```bash
conda activate geant-root-env
```

---

## Paso 3: Instalar Geant4 desde Conda

Con el entorno activo:

```bash
conda install -c conda-forge geant4
```

---

## Verificar que funciona

### Ver ROOT

```bash
root
```

Debe mostrar la bienvenida de ROOT. Sal con `.q` o Ctrl+D.

### Ver Geant4

```bash
geant4-config --version
```

Debe mostrar `11.3.2` u otra versión reciente.

---

## Paso 4: Probar visualización en Geant4

### 4.1 Código de ejemplo (`main.cc`)

Crea un archivo `main.cc` con el siguiente contenido:

```cpp
#include "G4RunManager.hh"
#include "G4UImanager.hh"
#include "G4NistManager.hh"
#include "G4Box.hh"
#include "G4LogicalVolume.hh"
#include "G4PVPlacement.hh"
#include "G4SystemOfUnits.hh"
#include "G4VUserDetectorConstruction.hh"
#include "FTFP_BERT.hh"
#include "G4UIExecutive.hh"
#include "G4VisExecutive.hh"

class MyDetectorConstruction : public G4VUserDetectorConstruction {
public:
    G4VPhysicalVolume* Construct() override {
        G4NistManager* nist = G4NistManager::Instance();
        G4Material* air = nist->FindOrBuildMaterial("G4_AIR");

        G4Box* solidWorld = new G4Box("World", 1.*m, 1.*m, 1.*m);
        G4LogicalVolume* logicWorld = new G4LogicalVolume(solidWorld, air, "World");
        return new G4PVPlacement(nullptr, {}, logicWorld, "World", nullptr, false, 0, true);
    }
};

int main(int argc, char** argv) {
    G4UIExecutive* ui = new G4UIExecutive(argc, argv);
    auto runManager = new G4RunManager();
    runManager->SetUserInitialization(new MyDetectorConstruction());
    runManager->SetUserInitialization(new FTFP_BERT());
    G4VisManager* visManager = new G4VisExecutive();
    visManager->Initialize();
    runManager->Initialize();

    G4UImanager* UImanager = G4UImanager::GetUIpointer();
    UImanager->ApplyCommand("/vis/open OGL");
    UImanager->ApplyCommand("/vis/drawVolume");
    UImanager->ApplyCommand("/vis/viewer/setViewpointThetaPhi 90 0");
    UImanager->ApplyCommand("/vis/scene/add/axes 0 0 0 1 m");

    ui->SessionStart();

    delete ui;
    delete visManager;
    delete runManager;
    return 0;
}
```

### 4.2 Compilar

```bash
g++ main.cc -o testG4 $(geant4-config --cflags --libs) -std=c++17
```

### 4.3 Ejecutar

```bash
./testG4
```

Deberías ver una ventana 3D con el volumen del mundo.

---

## Extras útiles

### Exportar tu entorno Conda a un `.yml`:

```bash
conda env export > geant-root-env.yml
```

Y puedes recrearlo así:

```bash
conda env create -f geant-root-env.yml
```

---

## Próximos pasos sugeridos

* Añadir partículas con `G4ParticleGun`
* Guardar datos con ROOT (`TTree`, `TFile`)
* Explorar `pyroot` y `jupyter notebook`
* Compilar Geant4 de forma externa si necesitas personalización avanzada



```markdown
---
title: Instalacion Root y Geant4 en Entorno Conda
published: 2025-10-04T04:40:26.381Z
tags: [Markdown, Blogging, Demo]
category: Root
draft: false
---
