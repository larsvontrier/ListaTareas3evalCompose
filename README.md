# Lista Tareas en Compose (examen de la tercera evaluación)

Este proyecto es la **plantilla de proyecto de Compose** del examen de la tercera evaluación de la asignatura _Programación Multimedia y Dispositivos Móviles (PMDMD)_ del ciclo formativo de Desarrollo de Aplicaciones Multiplataforma (DAM) del IES San Clemente.

<!-- El enunciado del examen puedes consultar en el siguiente enlace:

https://manuais.pages.iessanclemente.net/plantillas/dam/pmdmd/99exames/03exame3eval/index.html

-->

## Ejercicio. Lista de Tareas en Android con Compose

Se trata de implementar una aplicación Android que gestione una lista de tareas usando Jetpack Compose y ViewModel, siguiendo la estructura de clases especificada.

### Clases

#### 2. Clase `TareasViewModel`

Aunque **no se precisa del método fun getTareaById(id: Int)**, pues no hay navegación entre pantallas, **puedes emplear el ViewModel del ejercicio anterior**. Por ello, no se evalúa esta clase, pudiento emplear remember para trabajar con el estado de la lista de tareas.

- **Responsabilidades**:
    - Mantener la lista de tareas
    - Proporcionar métodos para:
        - `addTarea(descripcion: String) `
        - `setCompletada(id: Int, completada: Boolean)`
        - `deleteTarea(id: Int)`
- **Requisitos**:
    - Heredar de ViewModel
    - Usar `mutableStateListOf` para la lista observable o **trabajar con StateFlow del ViewModel anterior** (recomendado)
    - Manejar correctamente el estado

#### 3. Composable `TareaItem`

_1,5 puntos_

- **Parámetros**:
    - `tarea: Tarea` // tarea a mostrar
    - `onCheckedChange: (Boolean) -> Unit` // función para manejar el cambio de estado del checkbox
    - `onDelete: () -> Unit` // función para manejar el borrado de la tarea

![TareaItem](media/tareaitem.svg?width=500)

**UI Requerida**:

- Diseño en fila (`Row`)
- Checkbox marcado si está completado
- Texto de la tarea (tachado si está completada):

```kotlin
Text(
    text = tarea.descripcion,
    modifier = Modifier.weight(1f),
    style = if (tarea.completada) {
        MaterialTheme.typography.bodyMedium.copy(
            textDecoration = TextDecoration.LineThrough, // Tachado
            color = MaterialTheme.colorScheme.onSurface.copy(alpha = 0.5f)
        )
    } else {
        MaterialTheme.typography.bodyMedium
    }
)
```
- Icono de borrado (usar _Icons.Default.Delete_):

```kotlin
IconButton(onClick = onDelete) {
    Icon(Icons.Default.Delete, contentDescription = "Eliminar")
}
```

### 4. Composable `ListaTareasScreen`
_2 puntos_

Guarda el **estado del texto de la descripción** (`nuevaTarea`) de la tarea en un `mutableStateOf` o `rememberSaveable` (recomendado) y conecta el botón de añadir con el ViewModel.
El **estado de la lista de tareas se obtiene del ViewModel** y se observa para actualizar la UI cuando cambie.
La lista de tareas se mostrará en un `LazyColumn` y cada tarea se mostrará con el composable `TareaItem`.

- **UI Requerida**:
    - **Scaffold** con `TopAppBar` y mensaje de **Lista de tareas**
    - **Contenido del Scaffold en Column** con:
        - `Card` con: **Campo de texto (`OutlinedTextField`)** y **botón "Añadir" (`FilledIconButton`) una nueva tarea**.
        - Lista (`LazyColumn`) de componentes `TareaItem`.

- **Lógica**:
    - Eventos/acciones:
        - `onClick` del botón se le pasa `viewModel.addTarea(nuevaTarea)`
        - `onDelete` de `TareaItem` se le pasa `viewModel.deleteTarea(tarea.id)`
        - `onCheckedChange` de `TareaItem` se le pasa `viewModel.setCompletada(tarea.id, !tarea.completada)`
    - Manejar el estado del texto de entrada
    - ~~Gestionar el teclado (acción Done)~~


### Rúbrica del ejercicio 2

| Componente              | Puntos   | Criterios de Evaluación |
|-------------------------|----------|-------------------------|
|-------------------------| -------- |-------------------------|
| **TareasViewModel**     | -        | - Hereda de ViewModel<br>- Usa mutableStateListOf o StateFlow<br>- Métodos para añadir, eliminar y marcar tareas como completadas |
| **TareaItem**           | 1.5      | - Parámetros correctos<br>- Diseño en fila con Checkbox y texto<br>- Icono de borrado |
| **ListaTareasScreen**   | 2.0      | - Scaffold con TopAppBar y mensaje<br>- Campo de texto y botón "Añadir"<br>- LazyColumn con TareaItem<br>- Eventos/acciones correctamente implementados |
| **Total**               | 3.5      |                         |

## Entrega

Para la preparación de la entrega del examen, el proyecto debe estar comprimido **sin los archivo de compilación (build)** y con el nombre correcto. Los archivos de compilación pueden aumentar significativamente el tamaño del archivo, por lo que es importante eliminarlos antes de la entrega. Además de no ser necesarios.

- Desde Android Studio, **selecciona el proyecto de Compose** que has creado para el examen. 
- Exporta el proyecto a un archivo comprimido `.zip` o `.7z` que contenga ambos proyectos: el de Compose y el de ViewModel con navegación.
- Cambia el nombre del archivo a **`Apellido1Apellido2Nombre3Eval.zip`** (o `.7z` si prefieres ese formato).
- Existen dos métodos para **crear el archivo comprimido** (debes hacerlo con ambos proyectos):

### Desde Android Studio

- Ve a **File -> Export to Zip File...** y selecciona la ubicación donde guardar el archivo comprimido.
- Asegúrate de que el archivo comprimido contenga el proyecto.

### Método manual

- Navega al directorio del proyecto en tu sistema de archivos (usualmente el escritorio en el caso de exámenes).
- Navega hasta el directorio app/ del proyecto y borra el directorio `build/` si existe (en algún caso podría haber un directorio `build/` en la raíz del proyecto, lo dudo, pero bórralo si existe).
- Haz clic derecho y selecciona "Comprimir" o "Añadir al archivo..." dependiendo de tu sistema operativo.

> Importante: **haz una copia en local y ==borra el directorio `build` de ambos proyectos que están en el directorio raíz del proyecto (no va a existir, en principio) y/o en el directorio app/==**. **No podrás enviar el archivo si no lo haces**, pues el **tamaño del archivo superará el límite permitido**==. Debería borrarse:
    
    _- **`build/`**: si existe, este directorio contiene los archivos generados durante la compilación._
    
    - **`app/build/`**: igual que el build/ en la raíz del proyecto, este contiene los archivos de compilación específicos de la aplicación.
    
Debes hacer dos entregas, **habiendo comprimido el archivo en un `.zip` ambos proyectos**
    
Pulsando el ratón y la tecla Ctrl sobre la **carpeta del proyecto de Android Studio -> Botón derecho ->** **_7-zip -> Engadir ao arquivo...._**. Os ayudaré si tenéis problemas.

El formato de fichero debe ser:

`_Apellido1Apellido2Nombre23Eval.zip (o *.7z)_`

A) **En la ==carpeta compartida==**: **`S:\_Materiais_Exames\02DAMMD-PMDM_entregas`** entrega el proyecto completo en un archivo `.zip` con tu nombre y apellidos en el nombre del archivo: **`Apellido1Apellido2Nombre3Eval.zip`**.

B) En el **aula virtual**, **sube el archivo `.zip`** a través de la tarea correspondiente:

**Comprime el proyecto en un archivo `.zip`** y **envíalo a través del aula virtual**.

