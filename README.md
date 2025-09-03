# ClaseIV

import sys
from PyQt5.QtWidgets import QApplication, QMainWindow, QTextEdit, QAction, QFileDialog, QMessageBox, QStatusBar
from PyQt5.QtGui import QKeySequence


class EditorTexto(QMainWindow):
    def __init__(self):
        super().__init__()
        self.setWindowTitle("Editor de Texto con Fondo")
        self.setGeometry(100, 100, 800, 600)
        self.setStyleSheet("""
   
    QMainWindow {
        border-image: url(rosas.png) 0 0 0 0 stretch stretch;
    }
)

    QMenuBar {
        background-color: #2c3e50;  
        color: white;              
    }
    QMenuBar::item {
        background: transparent;
        padding: 4px 12px;
    }
    QMenuBar::item:selected {
        background: #007bff;        
        color: white;
    }

    
    QMenu {
        background-color: #ffffff;  
        border: 1px solid #ccc;     
        border-radius: 5px;        
    }
    QMenu::item {
        padding: 5px 20px;        
    }
    QMenu::item:selected {
        background-color: #007bff; 
        color: white;
    }
""")



        # --- Área de texto ---
        self.editor = QTextEdit()
        self.setCentralWidget(self.editor)
        self.editor.setPlaceholderText("Escribe aquí tu texto...")

        # 🎨 Editor transparente
        self.editor.setStyleSheet('''
        QTextEdit {
        background: transparent;   
        font-family: 'Georgia';
        font-size: 14pt;
        color: #000000;  
        padding: 15px;
        border: none;   
            }
''')



    # Crear menús y barra de estado
        self.crear_menus()
        self.crear_barra_estado()


    # Menús
 
    def crear_menus(self):
        menubar = self.menuBar()

        # Menú Archivo
        menu_archivo = menubar.addMenu("&Archivo")

        accion_nuevo = QAction("&Nuevo", self)
        accion_nuevo.setShortcut(QKeySequence.New)
        accion_nuevo.triggered.connect(self.nuevo_archivo)
        menu_archivo.addAction(accion_nuevo)

        accion_abrir = QAction("&Abrir", self)
        accion_abrir.setShortcut(QKeySequence.Open)
        accion_abrir.triggered.connect(self.abrir_archivo)
        menu_archivo.addAction(accion_abrir)

        accion_guardar = QAction("&Guardar", self)
        accion_guardar.setShortcut(QKeySequence.Save)
        accion_guardar.triggered.connect(self.guardar_archivo)
        menu_archivo.addAction(accion_guardar)

        accion_salir = QAction("&Salir", self)
        accion_salir.setShortcut(QKeySequence.Quit)
        accion_salir.triggered.connect(self.salir)
        menu_archivo.addAction(accion_salir)

        # Menú Ayuda
        menu_ayuda = menubar.addMenu("&Ayuda")

        accion_acerca = QAction("Acerca de", self)
        accion_acerca.triggered.connect(self.acerca_de)
        menu_ayuda.addAction(accion_acerca)

   
    # Funciones de archivo
   
    def nuevo_archivo(self):
        self.editor.clear()
        self.statusBar().showMessage("Nuevo documento")

    def abrir_archivo(self):
        archivo, _ = QFileDialog.getOpenFileName(self, "Abrir archivo", "", "Archivos de texto (*.txt)")
        if archivo:
            try:
                with open(archivo, "r", encoding="utf-8") as f:
                    contenido = f.read()
                    self.editor.setPlainText(contenido)
                self.statusBar().showMessage(f"Archivo abierto: {archivo}")
            except Exception as e:
                QMessageBox.warning(self, "Error", f"No se pudo abrir el archivo:\n{e}")

    def guardar_archivo(self):
        archivo, _ = QFileDialog.getSaveFileName(self, "Guardar archivo", "", "Archivos de texto (*.txt)")
        if archivo:
            try:
                with open(archivo, "w", encoding="utf-8") as f:
                    f.write(self.editor.toPlainText())
                self.statusBar().showMessage(f"Archivo guardado: {archivo}")
            except Exception as e:
                QMessageBox.warning(self, "Error", f"No se pudo guardar el archivo:\n{e}")

    # Diálogos
    
    def acerca_de(self):
        QMessageBox.information(
            self, "Acerca de",
            "Editor de Texto v2.0\n\nCon fondo personalizado y estilo propio.\nCreado con PyQt5."
        )

    def salir(self):
        respuesta = QMessageBox.question(
            self, "Salir",
            "¿Desea guardar los cambios antes de salir?",
            QMessageBox.Yes | QMessageBox.No | QMessageBox.Cancel
        )
        if respuesta == QMessageBox.Yes:
            self.guardar_archivo()
            self.close()
        elif respuesta == QMessageBox.No:
            self.close()
       

    # Barra de estado
   
    def crear_barra_estado(self):
        self.setStatusBar(QStatusBar())
        self.statusBar().showMessage("Listo")


if __name__ == "__main__":
    app = QApplication(sys.argv)
    editor = EditorTexto()
    editor.show()
    sys.exit(app.exec_())
