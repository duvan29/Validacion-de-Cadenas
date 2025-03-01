# Documentación del Proyecto - Validación de Cadenas en Python

## Integrantes del Grupo
- Duvan Camilo Serrano Rojas
- Nicolas Santiago Cancimance Pabon
- Cristian Camilo Gomez Arenas

## 1. Expresiones Regulares Utilizadas
A continuación, se presentan las expresiones regulares utilizadas en el proyecto para la validación de distintos tipos de datos:

- **Números enteros (positivos y negativos):**
  ```regex
  ^[-+]?\d+$
  ```
- **Números reales (positivos y negativos, con punto decimal):**
  ```regex
  ^[-+]?\d+\.\d+$
  ```
- **Notación científica:**
  ```regex
  ^[-+]?\d+(\.\d+)?[eE][-+]?\d+$
  ```
- **Correo electrónico:**
  ```regex
  ^[\w.-]+@[\w.-]+\.[a-zA-Z]{2,}$
  ```

## 2. Diagramas UML
### 2.1 Diagrama de Clases
El sistema sigue el patrón **Modelo-Vista-Controlador (MVC)**, con las siguientes clases principales:

- **Modelo:** Contiene la lógica de validación mediante expresiones regulares.
- **Vista:** Proporciona la interfaz gráfica de usuario (GUI) con los campos de entrada y botones.
- **Controlador:** Maneja la comunicación entre la vista y el modelo, procesando las validaciones.

_(Incluir aquí una imagen del diagrama de clases)_

### 2.2 Diagrama de Secuencia
Este diagrama muestra la interacción entre los componentes durante la validación de una entrada.

_(Incluir aquí una imagen del diagrama de secuencia)_

### 2.3 Diagrama de Estados
Muestra los posibles estados del sistema y las transiciones entre ellos.

_(Incluir aquí una imagen del diagrama de estados)_

## 3. Código Fuente
A continuación, se presenta el código fuente del proyecto en Python:

```python
import tkinter as tk
import re

# Modelo: Encapsula las expresiones regulares y la validación de cada tipo de dato.
class RegexModel:
    def __init__(self):
        # Patrón para número entero (con signo opcional)
        self.pattern_int = re.compile(r'^[+-]?\d+$')
        # Patrón para número real (con punto decimal y signo opcional)
        self.pattern_float = re.compile(r'^[+-]?\d+\.\d+$')
        # Patrón para notación científica (ejemplo: 3.14e-10)
        self.pattern_scientific = re.compile(r'^[+-]?(\d+(\.\d+)?)[eE][+-]?\d+$')
        # Patrón simple para correo electrónico
        self.pattern_email = re.compile(r'^[\w\.-]+@[\w\.-]+\.\w+$')
    
    def validar_entero(self, texto):
        return bool(self.pattern_int.fullmatch(texto))
    
    def validar_real(self, texto):
        return bool(self.pattern_float.fullmatch(texto))
    
    def validar_cientifico(self, texto):
        return bool(self.pattern_scientific.fullmatch(texto))
    
    def validar_email(self, texto):
        return bool(self.pattern_email.fullmatch(texto))

# Vista: Interfaz gráfica utilizando Tkinter.
class RegexView(tk.Frame):
    def __init__(self, master=None):
        super().__init__(master)
        self.master = master
        self.master.title("Validador de Expresiones Regulares")
        self.create_widgets()

    def create_widgets(self):
        # Etiqueta y campo para número entero
        self.lbl_entero = tk.Label(self.master, text="Número Entero:")
        self.lbl_entero.grid(row=0, column=0, padx=5, pady=5, sticky="e")
        self.entry_entero = tk.Entry(self.master, width=30)
        self.entry_entero.grid(row=0, column=1, padx=5, pady=5)

        # Etiqueta y campo para número real
        self.lbl_real = tk.Label(self.master, text="Número Real:")
        self.lbl_real.grid(row=1, column=0, padx=5, pady=5, sticky="e")
        self.entry_real = tk.Entry(self.master, width=30)
        self.entry_real.grid(row=1, column=1, padx=5, pady=5)

        # Etiqueta y campo para notación científica
        self.lbl_cientifico = tk.Label(self.master, text="Notación Científica:")
        self.lbl_cientifico.grid(row=2, column=0, padx=5, pady=5, sticky="e")
        self.entry_cientifico = tk.Entry(self.master, width=30)
        self.entry_cientifico.grid(row=2, column=1, padx=5, pady=5)

        # Etiqueta y campo para correo electrónico
        self.lbl_email = tk.Label(self.master, text="Correo Electrónico:")
        self.lbl_email.grid(row=3, column=0, padx=5, pady=5, sticky="e")
        self.entry_email = tk.Entry(self.master, width=30)
        self.entry_email.grid(row=3, column=1, padx=5, pady=5)

        # Botón para realizar la validación
        self.btn_validar = tk.Button(self.master, text="Validar", command=self.on_validar)
        self.btn_validar.grid(row=4, column=0, columnspan=2, pady=10)

        # Etiqueta para mostrar los resultados de la validación
        self.lbl_resultados = tk.Label(self.master, text="", fg="blue")
        self.lbl_resultados.grid(row=5, column=0, columnspan=2, pady=5)

    def on_validar(self):
        # Delegamos la acción al controlador
        if self.master.controller:
            self.master.controller.validar_datos()

    def mostrar_resultados(self, resultados):
        # Muestra los mensajes de validación en la etiqueta de resultados
        mensaje = ""
        for campo, resultado in resultados.items():
            mensaje += f"{campo}: {resultado}\n"
        self.lbl_resultados.config(text=mensaje)

# Controlador: Coordina la interacción entre la vista y el modelo.
class RegexController:
    def __init__(self, view, model):
        self.view = view
        self.model = model

    def validar_datos(self):
        # Se obtienen los datos ingresados en cada campo
        datos = {
            "Entero": self.view.entry_entero.get(),
            "Real": self.view.entry_real.get(),
            "Científico": self.view.entry_cientifico.get(),
            "Email": self.view.entry_email.get()
        }
        resultados = {}
        # Validación del campo entero
        if self.model.validar_entero(datos["Entero"]):
            resultados["Entero"] = "Correcto"
        else:
            resultados["Entero"] = "Error en el número entero"
        # Validación del campo real
        if self.model.validar_real(datos["Real"]):
            resultados["Real"] = "Correcto"
        else:
            resultados["Real"] = "Error en el número real"
        # Validación del campo notación científica
        if self.model.validar_cientifico(datos["Científico"]):
            resultados["Científico"] = "Correcto"
        else:
            resultados["Científico"] = "Error en la notación científica"
        # Validación del campo correo electrónico
        if self.model.validar_email(datos["Email"]):
            resultados["Email"] = "Correcto"
        else:
            resultados["Email"] = "Error en el correo electrónico"
        
        # Muestra los resultados en la vista
        self.view.mostrar_resultados(resultados)

def main():
    root = tk.Tk()
    app = RegexView(master=root)
    model = RegexModel()
    controller = RegexController(app, model)
    # Se asocia el controlador al root para poder ser llamado desde la vista
    root.controller = controller
    app.mainloop()

if __name__ == "__main__":
    main()
```

## 4. Instrucciones de Ejecución
Ejecutar el archivo `python autof.py` para iniciar la aplicación.
