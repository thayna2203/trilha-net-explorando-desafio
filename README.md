# DIO - Trilha .NET - Explorando a linguagem C#
www.dio.me

## Desafio de projeto
Para este desafio, você precisará usar seus conhecimentos adquiridos no módulo de explorando a linguagem C#, da trilha .NET da DIO.

## Contexto
Você foi contratado para construir um sistema de hospedagem, que será usado para realizar uma reserva em um hotel. Você precisará usar a classe Pessoa, que representa o hóspede, a classe Suíte, e a classe Reserva, que fará um relacionamento entre ambos.

O seu programa deverá cálcular corretamente os valores dos métodos da classe Reserva, que precisará trazer a quantidade de hóspedes e o valor da diária, concedendo um desconto de 10% para caso a reserva seja para um período maior que 10 dias.

## Regras e validações
1. Não deve ser possível realizar uma reserva de uma suíte com capacidade menor do que a quantidade de hóspedes. Exemplo: Se é uma suíte capaz de hospedar 2 pessoas, então ao passar 3 hóspedes deverá retornar uma exception.
2. O método ObterQuantidadeHospedes da classe Reserva deverá retornar a quantidade total de hóspedes, enquanto que o método CalcularValorDiaria deverá retornar o valor da diária (Dias reservados x valor da diária).
3. Caso seja feita uma reserva igual ou maior que 10 dias, deverá ser concedido um desconto de 10% no valor da diária.


![Diagrama de classe estacionamento](diagrama_classe_hotel.png)

## Solução
O código está pela metade, e você deverá dar continuidade obedecendo as regras descritas acima, para que no final, tenhamos um programa funcional. Procure pela palavra comentada "TODO" no código, em seguida, implemente conforme as regras acima.
/HotelReserva
 ├── Models
 │   ├── Pessoa.cs
 │   ├── Suite.cs
 │   └── Reserva.cs
 ├── Program.cs
 └── HotelReserva.csproj
namespace HotelReserva.Models
{
    public class Pessoa
    {
        public Pessoa(string nome, string sobrenome)
        {
            Nome = nome;
            Sobrenome = sobrenome;
        }

        public string Nome { get; set; }
        public string Sobrenome { get; set; }

        public string NomeCompleto => $"{Nome} {Sobrenome}".ToUpper();
    }
}
namespace HotelReserva.Models
{
    public class Suite
    {
        public Suite(string tipoSuite, int capacidade, decimal valorDiaria)
        {
            TipoSuite = tipoSuite;
            Capacidade = capacidade;
            ValorDiaria = valorDiaria;
        }

        public string TipoSuite { get; set; }
        public int Capacidade { get; set; }
        public decimal ValorDiaria { get; set; }
    }
}
namespace HotelReserva.Models
{
    public class Reserva
    {
        public Reserva(int diasReservados)
        {
            DiasReservados = diasReservados;
        }

        public int DiasReservados { get; set; }
        public List<Pessoa> Hospedes { get; set; } = new List<Pessoa>();
        public Suite Suite { get; set; }

        public void CadastrarHospedes(List<Pessoa> hospedes)
        {
            if (Suite != null && hospedes.Count <= Suite.Capacidade)
            {
                Hospedes = hospedes;
            }
            else
            {
                throw new Exception("A quantidade de hóspedes excede a capacidade da suíte.");
            }
        }

        public void CadastrarSuite(Suite suite)
        {
            Suite = suite;
        }

        public int ObterQuantidadeHospedes()
        {
            return Hospedes.Count;
        }

        public decimal CalcularValorDiaria()
        {
            decimal valor = DiasReservados * Suite.ValorDiaria;

            // Desconto de 10% para reservas acima de 10 dias
            if (DiasReservados > 10)
                valor *= 0.9M;

            return valor;
        }
    }
}
﻿using HotelReserva.Models;

// Cria os hóspedes
List<Pessoa> hospedes = new List<Pessoa>()
{
    new Pessoa("Thayná", "Oliveira"),
    new Pessoa("Maria", "Silva")
};

// Cria a suíte
Suite suite = new Suite("Premium", 2, 100M);

// Cria a reserva
Reserva reserva = new Reserva(diasReservados: 12);
reserva.CadastrarSuite(suite);
reserva.CadastrarHospedes(hospedes);

// Exibe informações
Console.WriteLine($"Hóspedes: {reserva.ObterQuantidadeHospedes()}");
Console.WriteLine($"Valor diária: {reserva.CalcularValorDiaria()}");
dotnet new console -n HotelReserva
