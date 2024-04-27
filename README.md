# Otimizando-o-Sistema-Banc-rio-com-Fun-es-Python
Otimizando o Sistema Bancário com Funções Python
import random

class ContaBancaria:
    def __init__(self, cpf, nome, senha):
        self.cpf = cpf
        self.nome = nome
        self.senha = senha
        self.saldo = 0
        self.limite_saldo = 5000   # Limite de saldo inicial
        self.limite_saque = 500    # Limite de saque inicial

    def depositar(self, valor):
        if valor > 0:
            if self.saldo + valor > self.limite_saldo:
                print("Limite de saldo excedido.")
            else:
                self.saldo += valor
                print("Depósito de R$ {:.2f} realizado com sucesso.".format(valor))
                self.mostrar_saldo()
        else:
            print("Valor de depósito inválido.")

    def sacar(self, valor):
        if valor <= 0:
            print("Valor de saque inválido.")
        elif valor > self.limite_saque:
            print("Valor de saque excede o limite permitido.")
        elif valor > self.saldo:
            print("Saldo insuficiente.")
        else:
            self.saldo -= valor
            print("Saque de R$ {:.2f} realizado com sucesso.".format(valor))
            self.mostrar_saldo()

    def ver_extrato(self):
        print("--- Extrato ---")
        print("CPF: ", self.cpf)
        print("Nome: ", self.nome)
        print("Saldo atual: R$ {:.2f}".format(self.saldo))

    def mostrar_saldo(self):
        print("Saldo atual: R$ {:.2f}".format(self.saldo))

class Banco:
    def __init__(self):
        self.clientes = {}

    def criar_conta(self, cpf, nome, senha):
        if cpf not in self.clientes:
            conta = ContaBancaria(cpf, nome, senha)
            self.clientes[cpf] = conta
            print("Conta criada com sucesso para {}.".format(nome))
        else:
            print("CPF já registrado no sistema.")

    def fazer_login(self, cpf, senha):
        if cpf in self.clientes:
            conta = self.clientes[cpf]
            if conta.senha == senha:
                print("Login bem-sucedido.")
                return conta
            else:
                print("Senha incorreta.")
        else:
            print("CPF não registrado no sistema.")
        return None

def main():
    banco = Banco()

    while True:
        print("\n--- Sistema Bancário ---")
        print("A. Criar Conta")
        print("B. Fazer Login")
        print("C. Ver Extrato")
        print("D. Saldo")
        print("E. Depositar")
        print("F. Sacar")
        print("G. Sair")

        opcao = input("Escolha uma opção: ").upper()

        if opcao == "A":
            cpf = input("Digite seu CPF: ")
            nome = input("Digite seu nome: ")
            senha = input("Digite sua senha: ")
            banco.criar_conta(cpf, nome, senha)
        elif opcao == "B":
            cpf = input("Digite seu CPF: ")
            senha = input("Digite sua senha: ")
            cliente = banco.fazer_login(cpf, senha)
            if cliente:
                print("Bem-vindo, {}!".format(cliente.nome))
        elif opcao == "C":
            if cliente:
                cliente.ver_extrato()
            else:
                print("Faça login primeiro.")
        elif opcao == "D":
            if cliente:
                cliente.mostrar_saldo()
            else:
                print("Faça login primeiro.")
        elif opcao == "E":
            if cliente:
                valor = float(input("Digite o valor a ser depositado: "))
                cliente.depositar(valor)
            else:
                print("Faça login primeiro.")
        elif opcao == "F":
            if cliente:
                valor = float(input("Digite o valor a ser sacado: "))
                cliente.sacar(valor)
            else:
                print("Faça login primeiro.")
        elif opcao == "G":
            print("Saindo do sistema bancário...")
            break
        else:
            print("Opção inválida.")

if __name__ == "__main__":
    main()
