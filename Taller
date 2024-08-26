using System;
using System.Collections.Generic;
using System.IO;
using System.Linq;

public class Producto
{
    public int Id { get; set; }
    public string Nombre { get; set; }
    public decimal Precio { get; set; }

    public Producto(int id, string nombre, decimal precio)
    {
        Id = id;
        Nombre = nombre;
        Precio = precio;
    }

    public override string ToString()
    {
        return $"ID: {Id}, Nombre: {Nombre}, Precio: {Precio:C}";
    }
}

public class Mesa
{
    public int Numero { get; set; }
    public List<Producto> Productos { get; set; }

    public Mesa(int numero)
    {
        Numero = numero;
        Productos = new List<Producto>();
    }

    public void AgregarProducto(Producto producto)
    {
        Productos.Add(producto);
    }

    public void EliminarProducto(int id)
    {
        Productos.RemoveAll(p => p.Id == id);
    }

    public decimal CalcularTotal()
    {
        return Productos.Sum(p => p.Precio);
    }
}

public class Factura
{
    public int Numero { get; set; }
    public DateTime Fecha { get; set; }
    public List<Producto> Productos { get; set; }

    public Factura(int numero)
    {
        Numero = numero;
        Fecha = DateTime.Now;
        Productos = new List<Producto>();
    }

    public void AgregarProducto(Producto producto)
    {
        Productos.Add(producto);
    }

    public void Imprimir()
    {
        Console.WriteLine($"Factura N°: {Numero}");
        Console.WriteLine($"Fecha: {Fecha}");
        foreach (var producto in Productos)
        {
            Console.WriteLine($"{producto.Nombre} = {producto.Precio:C}");
        }
        Console.WriteLine($"Total: {CalcularTotal():C}");
    }

    public decimal CalcularTotal()
    {
        return Productos.Sum(p => p.Precio);
    }
}

public class Sistema
{
    public List<Producto> Productos { get; private set; }
    public List<Mesa> Mesas { get; private set; }
    public List<Factura> Facturas { get; private set; }

    public Sistema()
    {
        Productos = new List<Producto>();
        Mesas = new List<Mesa>();
        Facturas = new List<Factura>();
    }

    public void ImprimirMenu()
    {
        Console.WriteLine("+---------------------------+");
        Console.WriteLine("|           MENÚ            |");
        Console.WriteLine("+---------------------------+");
        foreach (var producto in Productos)
        {
            Console.WriteLine(producto);
        }
        Console.WriteLine("+---------------------------+");
    }

    public void AgregarProductoAMesa(int mesaNumero, Producto producto)
    {
        var mesa = Mesas.FirstOrDefault(m => m.Numero == mesaNumero);
        if (mesa != null)
        {
            mesa.AgregarProducto(producto);
        }
    }

    public void ImprimirCuenta(int mesaNumero)
    {
        var mesa = Mesas.FirstOrDefault(m => m.Numero == mesaNumero);
        if (mesa != null)
        {
            Console.WriteLine("+---------------------------+");
            Console.WriteLine("|         FACTURA           |");
            Console.WriteLine("+---------------------------+");
            Console.WriteLine($"Factura N°: {mesaNumero}");
            Console.WriteLine($"Fecha: {DateTime.Now}");
            foreach (var producto in mesa.Productos)
            {
                Console.WriteLine($"{producto.Nombre} = {producto.Precio:C}");
            }
            Console.WriteLine($"Total: {mesa.CalcularTotal():C}");
            Console.WriteLine("+---------------------------+");
        }
    }

    public void CargarDatos(string ruta)
    {
        if (File.Exists(ruta))
        {
            var lines = File.ReadAllLines(ruta);
            Productos.Clear();
            foreach (var line in lines)
            {
                var parts = line.Split(';');
                var id = int.Parse(parts[0]);
                var nombre = parts[1];
                var precio = decimal.Parse(parts[2]);
                Productos.Add(new Producto(id, nombre, precio));
            }
        }
    }

    public void GuardarDatos(string ruta)
    {
        using (var writer = new StreamWriter(ruta))
        {
            foreach (var producto in Productos)
            {
                writer.WriteLine($"{producto.Id};{producto.Nombre};{producto.Precio}");
            }
        }
    }
}

class Program
{
    static void Main(string[] args)
    {
        var sistema = new Sistema();

        // Inicializar algunos productos
        sistema.Productos.Add(new Producto(1, "Pizza", 10.99m));
        sistema.Productos.Add(new Producto(2, "Coca Cola", 1.99m));
        sistema.Productos.Add(new Producto(3, "Ensalada", 5.99m));

        // Agregar mesas
        sistema.Mesas.Add(new Mesa(1));
        sistema.Mesas.Add(new Mesa(2));

        while (true)
        {
            Console.WriteLine("1. Imprimir Menú");
            Console.WriteLine("2. Agregar Producto a Mesa");
            Console.WriteLine("3. Imprimir Cuenta de Mesa");
            Console.WriteLine("4. Cargar Datos");
            Console.WriteLine("5. Guardar Datos");
            Console.WriteLine("6. Salir");
            var opcion = Console.ReadLine();

            switch (opcion)
            {
                case "1":
                    sistema.ImprimirMenu();
                    break;
                case "2":
                    Console.WriteLine("Ingrese número de mesa:");
                    int mesaNumero = int.Parse(Console.ReadLine());
                    Console.WriteLine("Ingrese ID del producto:");
                    int productoId = int.Parse(Console.ReadLine());
                    var producto = sistema.Productos.FirstOrDefault(p => p.Id == productoId);
                    if (producto != null)
                    {
                        sistema.AgregarProductoAMesa(mesaNumero, producto);
                    }
                    else
                    {
                        Console.WriteLine("Producto no encontrado.");
                    }
                    break;
                case "3":
                    Console.WriteLine("Ingrese número de mesa:");
                    mesaNumero = int.Parse(Console.ReadLine());
                    sistema.ImprimirCuenta(mesaNumero);
                    break;
                case "4":
                    Console.WriteLine("Ingrese ruta del archivo para cargar:");
                    string cargaRuta = Console.ReadLine();
                    sistema.CargarDatos(cargaRuta);
                    break;
                case "5":
                    Console.WriteLine("Ingrese ruta del archivo para guardar:");
                    string guardarRuta = Console.ReadLine();
                    sistema.GuardarDatos(guardarRuta);
                    break;
                case "6":
                    return;
                default:
                    Console.WriteLine("Opción no válida.");
                    break;
            }
        }
    }
}

