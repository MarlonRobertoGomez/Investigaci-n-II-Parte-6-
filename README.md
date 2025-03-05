Facade
using System;

// 1️⃣ Subsistema: Clases complejas con diferentes responsabilidades
class StockService
{
    public bool VerificarStock(string producto, int cantidad)
    {
        Console.WriteLine($"Verificando stock para {producto} ({cantidad} unidades)...");
        return cantidad <= 10; // Simulación: Hay stock si la cantidad es <= 10
    }
}

class PagoService
{
    public bool ProcesarPago(string tarjeta, decimal monto)
    {
        Console.WriteLine($"Procesando pago de ${monto} con tarjeta {tarjeta}...");
        return true; // Simulación: Siempre aprueba el pago
    }
}

class FacturaService
{
    public void GenerarFactura(string cliente, decimal monto)
    {
        Console.WriteLine($"Generando factura para {cliente} por ${monto}...");
    }
}

// 2️⃣ Fachada: Encapsula la lógica compleja en una interfaz simple
class PedidoFacade
{
    private StockService _stock;
    private PagoService _pago;
    private FacturaService _factura;

    public PedidoFacade()
    {
        _stock = new StockService();
        _pago = new PagoService();
        _factura = new FacturaService();
    }

    public void RealizarPedido(string producto, int cantidad, string tarjeta, string cliente)
    {
        Console.WriteLine("\n🔹 Iniciando proceso de pedido...");

        if (!_stock.VerificarStock(producto, cantidad))
        {
            Console.WriteLine("❌ Pedido cancelado: No hay suficiente stock.");
            return;
        }

        if (!_pago.ProcesarPago(tarjeta, cantidad * 20)) // Precio ficticio: $20 por unidad
        {
            Console.WriteLine("❌ Pedido cancelado: Pago no aprobado.");
            return;
        }

        _factura.GenerarFactura(cliente, cantidad * 20);
        Console.WriteLine("✅ Pedido realizado con éxito.");
    }
}

// 3️⃣ Cliente: Interactúa solo con la fachada
class Program
{
    static void Main()
    {
        PedidoFacade pedido = new PedidoFacade();

        // Cliente hace un pedido sin preocuparse por los detalles internos
        pedido.RealizarPedido("Laptop", 2, "1234-5678-9876-5432", "Juan Pérez");

        Console.ReadKey();
    }
}

Flyweight
using System;
using System.Collections.Generic;

// Interfaz Flyweight
public interface IShape
{
    void Draw(string color);
}

// Clase Concreta Flyweight (compartida)
public class Circle : IShape
{
    private readonly string _intrinsicState; // Estado compartido

    public Circle()
    {
        _intrinsicState = "Círculo";
    }

    public void Draw(string color)
    {
        Console.WriteLine($"Dibujando {_intrinsicState} con color {color}");
    }
}

// Flyweight Factory
public class ShapeFactory
{
    private readonly Dictionary<string, IShape> _shapes = new();

    public IShape GetCircle(string color)
    {
        if (!_shapes.ContainsKey(color))
        {
            _shapes[color] = new Circle();
            Console.WriteLine($"Creando nuevo círculo de color {color}");
        }
        return _shapes[color];
    }
}

// Cliente
class Program
{
    static void Main()
    {
        ShapeFactory factory = new ShapeFactory();

        IShape circle1 = factory.GetCircle("Rojo");
        circle1.Draw("Rojo");

        IShape circle2 = factory.GetCircle("Azul");
        circle2.Draw("Azul");

        IShape circle3 = factory.GetCircle("Rojo"); // Se reutiliza el objeto existente
        circle3.Draw("Rojo");

        Console.WriteLine("Cantidad de objetos en la fábrica: " + factory.GetCirclesCount());
    }
}
