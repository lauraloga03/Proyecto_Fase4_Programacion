import tkinter as tk
from tkinter import ttk, messagebox
from abc import ABC, abstractmethod
import datetime

# -------- LOGS --------
def registrar_log(mensaje):
    with open("logs.txt", "a") as archivo:
        archivo.write(f"{datetime.datetime.now()} - {mensaje}\n")

# -------- MODELO --------
class Servicio(ABC):
    def __init__(self, nombre, precio):
        self.nombre = nombre
        self.precio = precio

    @abstractmethod
    def calcular_costo(self, duracion):
        pass

class Sala(Servicio):
    def calcular_costo(self, horas):
        if horas <= 0:
            raise ValueError("Horas inválidas")
        return self.precio * horas

class Equipo(Servicio):
    def calcular_costo(self, dias):
        return self.precio * dias

class Asesoria(Servicio):
    def calcular_costo(self, horas):
        return self.precio * horas * 1.2

class Cliente:
    def __init__(self, nombre, documento):
        if not nombre or not documento:
            raise ValueError("Datos incompletos")
        self.nombre = nombre
        self.documento = documento

class Reserva:
    def __init__(self, cliente, servicio, duracion):
        self.cliente = cliente
        self.servicio = servicio
        self.duracion = duracion
        self.estado = "Activa"
        self.costo = servicio.calcular_costo(duracion)

# -------- APP --------
class App:
    def __init__(self, root):
        self.root = root
        self.root.title("Software FJ Pro")
        self.root.geometry("750x450")
        self.root.configure(bg="#1f1f2e")

        self.reservas = []

        # -------- SIDEBAR --------
        sidebar = tk.Frame(root, bg="#2a2a40", width=150)
        sidebar.pack(side="left", fill="y")

        tk.Label(sidebar, text="MENU", bg="#2a2a40", fg="white").pack(pady=10)

        tk.Button(sidebar, text="Nueva Reserva", command=self.vista_form).pack(pady=5)
        tk.Button(sidebar, text="Historial", command=self.vista_tabla).pack(pady=5)

        # -------- CONTENIDO --------
        self.contenido = tk.Frame(root, bg="#1f1f2e")
        self.contenido.pack(fill="both", expand=True)

        self.vista_form()

    # -------- FORMULARIO --------
    def vista_form(self):
        self.limpiar()

        tk.Label(self.contenido, text="Nueva Reserva", fg="white", bg="#1f1f2e", font=("Arial", 16)).pack(pady=10)

        self.nombre = tk.Entry(self.contenido)
        self.nombre.pack(pady=5)
        self.nombre.insert(0, "Nombre")

        self.documento = tk.Entry(self.contenido)
        self.documento.pack(pady=5)
        self.documento.insert(0, "Documento")

        self.duracion = tk.Entry(self.contenido)
        self.duracion.pack(pady=5)
        self.duracion.insert(0, "Duración")

        self.servicio = ttk.Combobox(self.contenido, values=["Sala", "Equipo", "Asesoria"])
        self.servicio.pack(pady=5)
        self.servicio.set("Sala")

        tk.Button(self.contenido, text="Guardar", command=self.guardar_reserva).pack(pady=10)

    # -------- TABLA --------
    def vista_tabla(self):
        self.limpiar()

        columnas = ("Cliente", "Servicio", "Duración", "Costo", "Estado")
        self.tabla = ttk.Treeview(self.contenido, columns=columnas, show="headings")

        for col in columnas:
            self.tabla.heading(col, text=col)

        self.tabla.pack(fill="both", expand=True)

        for r in self.reservas:
            self.tabla.insert("", "end", values=(
                r.cliente.nombre,
                r.servicio.nombre,
                r.duracion,
                r.costo,
                r.estado
            ))

        tk.Button(self.contenido, text="Cancelar Seleccion", command=self.cancelar).pack(pady=5)

    # -------- FUNCIONES --------
    def guardar_reserva(self):
        try:
            nombre = self.nombre.get()
            documento = self.documento.get()
            duracion = int(self.duracion.get())
            tipo = self.servicio.get()

            cliente = Cliente(nombre, documento)

            if tipo == "Sala":
                servicio = Sala("Sala", 50000)
            elif tipo == "Equipo":
                servicio = Equipo("Equipo", 30000)
            else:
                servicio = Asesoria("Asesoria", 80000)

            reserva = Reserva(cliente, servicio, duracion)
            self.reservas.append(reserva)

            messagebox.showinfo("OK", f"Reserva creada\nCosto: {reserva.costo}")

        except Exception as e:
            registrar_log(str(e))
            messagebox.showerror("Error", str(e))

    def cancelar(self):
        try:
            seleccion = self.tabla.selection()
            if not seleccion:
                raise ValueError("Seleccione una reserva")

            index = self.tabla.index(seleccion)
            self.reservas[index].estado = "Cancelada"
            self.vista_tabla()

        except Exception as e:
            registrar_log(str(e))
            messagebox.showerror("Error", str(e))

    def limpiar(self):
        for widget in self.contenido.winfo_children():
            widget.destroy()

# -------- EJECUCIÓN --------
root = tk.Tk()
app = App(root)
root.mainloop()