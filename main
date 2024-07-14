from datetime import datetime

class Cliente:
    def __init__(self, nome, cpf):
        self.nome = nome
        self.cpf = cpf

class Conta:
    def __init__(self, cliente, numero, saldo=0, tipo = '0'):
        self.cliente = cliente
        self.numero = numero
        self.saldo = saldo
        self.tipo = tipo
        self.historico = []
    
    def sacar(self, valor):
        if valor <= self.saldo:
            self.saldo -= valor
            self.registrar_operacao(f'Saque de R${valor:.2f} realizado!')
            print(f'Saque de R${valor:.2f} realizado com sucesso!')
        else:
            print('Saldo insuficiente para o saque!')
    
    def depositar(self, valor):
        self.saldo += valor
        self.registrar_operacao(f'Deposito de R${valor:.2f} realizado!')
        print(f'Deposito de R${valor:.2f} realizado com sucesso!')
    
    
    def transferir(self, valor, conta_destino):
        if valor <= self.saldo:
            self.saldo -= valor
            conta_destino.depositar(valor)
            self.registrar_operacao(f'Transferencia de R${valor:.2f} realizada para conta de {conta_destino.cliente.nome}!')
            print(f'Transferencia de R${valor:.2f} para conta de {conta_destino.cliente.nome} realizada com sucesso!')
        else:
            print('Saldo insuficiente para a transferencia!')
            
    def __str__(self):
        return "Dados da Conta: \nNúmero: {}\nTitular: {}\nSaldo: {}\nLimite: {}".format(self.numero, self.titular, self.saldo, self.limite)
    
    def extrato(self):
        print(f'cliente: {self.cliente.nome}')
        print(f'Conta: {self.numero} ({self.tipo})')
        print('Historico de Operações:')
        for operacao in self.historico:
            print(operacao)
        print(f'Saldo: R${self.saldo:.2f}')
    
    
    def registrar_operacao(self, descricao):
        data_hora = datetime.now()
        self.historico.append(f'{data_hora.strftime('%d/%m/%Y %H:%M:%S')} - {descricao}')

class ContaCorrente(Conta):
    
    def __init__(self, cliente, numero, saldo=0):
        super().__init__(cliente, numero, saldo, tipo='Conta Corrente')

    
    def atualizar(self, taxa):
        self.saldo += self.saldo * taxa * 2
        self.registrar_operacao(f'Taxa de {taxa}% aplicada (saldo adicional)')
        print(f'Taxa de {taxa}% aplicada (saldo adicional) com sucesso!')

    def depositar(self, valor):
        self.saldo += valor - 0.10
        self.registrar_operacao(f'Deposito de R${valor:.2f} realizado!')
        print(f'Deposito de R${valor:.2f} realizado com sucesso!')
        return self.saldo

class ContaPoupanca(Conta):
    
    def __init__(self, cliente, numero, saldo=0):
        super().__init__(cliente, numero, saldo, tipo='Conta Poupança')

    
    def atualizar(self, taxa):
        self.saldo += self.saldo * taxa * 3
        self.registrar_operacao(f'Taxa de {taxa}% aplicada (saldo adicional)')
        print(f'Taxa de {taxa}% aplicada (saldo adicional) com sucesso!')
        return self.saldo

class AtualizadorDeContas:    
    def __init__(self, selic, saldo_total=0):
        self._selic = selic
        self._saldo_total = saldo_total
    
    def roda(self, conta):
        print("Saldo da Conta: {}".format(conta.saldo))
        self._saldo_total += conta.atualiza(self._selic)
        print("Novo Saldo da Conta: {}".format(self._saldo_total))  



class Banco:
    def __init__(self):
        self.clientes = {}  
        self.contas = {}
        self.num_contas = 0   


    def cadastrar_cliente(self):
        nome = input('Informe o seu nome: ')
        cpf = input('Informe o seu CPF: ')
        if cpf in self.clientes:
            print('O cliente ja esta cadastrado!\n')
        else:
            cliente = Cliente(nome, cpf)
            self.clientes[cpf] = cliente
            print('\n-------------------------------------')
            print(f'Cliente {nome} cadastrado com sucesso!')


    def criar_conta(self):
        cpf = input('Informe o CPF do titular: ')
        if cpf in self.clientes:
            if cpf in self.contas:
                print('Cliente ja possui uma conta!')
            else:
                cliente = self.clientes[cpf]
                self.num_contas+=1
                tipo = input('Tipo da Conta: ')
                if tipo == '1':
                    conta = ContaPoupanca(cliente, self.num_contas)
                    self.contas[cpf] = conta
                    print('\n-------------------------------------')
                    print('Conta criada com sucesso!')
                elif tipo == '2':
                    conta = ContaCorrente(cliente, self.num_contas)
                    self.contas[cpf] = conta
                    print('\n-------------------------------------')
                    print('Conta criada com sucesso!') 
                    print(conta.tipo)
        else:
            print('\n-------------------------------------')
            print('Cliente nao encontrado!')


    def realizar_deposito(self):
        cpf = input('Informe o CPF do cliente da conta: ')
        if cpf in self.contas:
            conta = self.contas[cpf]
            valor = float(input('Informe o valor do deposito: '))
            conta.depositar(valor)
        else:
            print('Conta nao encontrada!')
  
    
    def realizar_saque(self):
        cpf = input('Informe o CPF do cliente da conta: ')
        if cpf in self.contas:
            conta = self.contas[cpf]
            valor = float(input('Informe o valor do saque: '))
            conta.sacar(valor)
        else:
            print('Conta nao encontrada!')
  
    
    def transferir(self):
        cpf_origem = input('Informe o CPF do cliente da conta de origem: ')
        if cpf_origem in self.contas:
            conta_origem = self.contas[cpf_origem]
            valor = float(input('Informe o valor da transferencia: '))
            if conta_origem.saldo >= valor:
                for cpf, cliente in self.clientes.items():
                    print(f'CPF: {cpf}, Nome: {cliente.nome}')
                
                cpf_destino = input('Informe o CPF do cliente da conta de destino: ')
                if cpf_destino in self.contas:
                    conta_destino = self.contas[cpf_destino]
                    conta_origem.sacar(valor)
                    conta_destino.depositar(valor)
                    print(f'Transferencia de R${valor:.2f} realizada com sucesso!')
                else:
                    print('Conta de destino nao encontrada!')
            else:
                print('Saldo insuficiente para a transferencia!')
        else:
            print('Conta de origem nao encontrada!')

    def atualizar(self):
        taxa = float(input('Informe a taxa de atualizacao do saldo: '))
        cpf = input('Informe o CPF do cliente da conta: ')
        for cpf in self.contas:
            conta = self.contas[cpf]
            conta.atualizar(taxa/100)
            print(f'Taxa de {taxa/100}% aplicada na conta de {conta.cliente.nome}!')
            
            
    def atualizador(self):
        selic = float(input('Informe a taxa de juros da selic: '))
        atualizador = AtualizadorDeContas(selic/100)

        for conta in self.contas.values():
            atualizador.roda(conta)
    
    def imprimir_extrato(self):
        cpf = input('Informe o CPF do cliente da conta: ')
        if cpf in self.contas:
            conta = self.contas[cpf]
            conta.extrato()
        else:
            print('Conta nao encontrada!')

    def menu(self):
        
        while True:
            try:
                print('\n\n====================================')
                print('Menu Banco')
                print('1 - Cadastrar Cliente')
                print('2 - Criar Conta')
                print('3 - Realizar Deposito')
                print('4 - Realizar Saque')
                print('5 - Transferir')
                print('6 - Ver Extrato')
                print('7 - Atualizar Conta')
                print('7 - Atualizar todas as Contas com a taxa selic')
                print('8 - Sair')
                print('====================================\n\n')

                opcao = int(input('Escolha uma opcao: '))

                if opcao == 1:
                    print('\n-------------------------------------')
                    self.cadastrar_cliente()
                    print('-------------------------------------')
                elif opcao == 2:
                    print('\n-------------------------------------')
                    self.criar_conta()
                    print('-------------------------------------')
                elif opcao == 3:
                    print('\n-------------------------------------')
                    self.realizar_deposito()
                    print('-------------------------------------')
                elif opcao == 4:
                    print('\n-------------------------------------')
                    self.realizar_saque()
                    print('-------------------------------------')
                elif opcao == 5:
                    print('\n-------------------------------------')
                    self.transferir()
                    print('-------------------------------------')
                elif opcao == 6:
                    print('\n-------------------------------------')
                    self.imprimir_extrato()
                    print('-------------------------------------')
                elif opcao == 7:
                    print('\n-------------------------------------')
                    self.atualizar()
                    print('-------------------------------------')
                elif opcao == 8:
                    print('\n-------------------------------------')
                    self.atualizador()
                    print('-------------------------------------')
                elif opcao == 9:
                    print('\n-------------------------------------')
                    print('Saindo do programa!')
                    print('-------------------------------------')
                    break
                else:
                    print('Opcao invalida. Tente novamente!')
            except ValueError:
                print('\n-------------------------------------')
                print('Insira um valor valido!')
                print('-------------------------------------')
                
banco = Banco()
banco.menu()
