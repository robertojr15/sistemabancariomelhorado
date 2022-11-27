# sistemabancariomelhorado
Evolução do sistema bancário 01

import textwrap

def menu():

    menu = """\n
    ================= MENU ================= 
    [d]\tDEPOSITAR
    [s]\tSACAR
    [e]\tEXTRATO
    [nc]\tNOVA CONTA
    [nu]\tNOVO USUÁRIO
    [q]\tSAIR
    => """
    return input(textwrap.dedent(menu))
def depositar(saldo, valor, extrato, /):
    if valor > 0:
        saldo += valor
        extrato += f"DEPÓSITO:\tR$ {valor:.2f}\n"
        print("\nDEPÓSITO REALIZADO COM SUCESSO!")
    else:
        print("\nOPERAÇÃO FALHOU! O VALOR INFORMADO É INVÁLIDO!")
    return saldo, extrato
def sacar(*, saldo, valor, extrato, limite, numero_saques, limite_saques):
    excedeu_saldo = valor > saldo
    excedeu_limite = valor > limite
    excedeu_saques = numero_saques >= limite_saques

    if excedeu_saldo:
        print("\nOPERAÇÃO FALHOU! VOCÊ NÃO TEM SALDO SUFICIENTE!")

    elif excedeu_limite:
        print("\nOPERAÇÃO FALHOU! O VALOR DO SAQUE EXCEDE O LIMITE!")

    elif excedeu_saques:
        print("\nOPERAÇÃO FALHOU! NÚMERO MÁXIMO DE SAQUES EXCEDIDO!")

    elif valor > 0:
        saldo -= valor
        extrato += f"SAQUE:\t\tR$ {valor:2f}\n"
        numero_saques += 1
        print("\nOPERAÇÃO FALHOU. O VALOR INFORMADO É INVÁLIDO!")

    return saldo, extrato
def exibir_extrato(saldo, /, *, extrato):
    print("\n========== EXTRATO ==========")
    print("NÃO FORAM REALIZADAS MOVIMENTAÇÕES." if not extrato else extrato)
    print(f"\nSALDO:\t\tR$ {saldo:.2f}")
    print("==============================")
def criar_usuario(usuarios):
    cpf = input("INFORME O CPF (SOMENTE NÚMEROS): ")
    usuario = filtrar_usuario(cpf, usuarios)

    if usuarios:
        print("\nJÁ EXISTE USUÁRIO COM ESSSE CPF!")
        return
    nome = input("INFORME O NOME COMPLETO: ")
    data_nascimento = input("INFORME A DATA DE NASCIMENTO (DD-MM-AAAA): ")
    endereco = input("INFORME O ENDEREÇO (RUA/AV, Nº - BAIRRO - CIDADE/UF): ")

    usuarios.append({"nome": nome, "data_nascimento": data_nascimento, "cpf": cpf, "endereco": endereco})

    print("USUÁRIO CRIADO COM SUCESSO!")
def filtrar_usuario(cpf, usuarios):
    usuarios_filtrados = [usuario for usuario in usuarios if usuario["cpf"] == cpf]
    return usuarios_filtrados[0] if usuarios_filtrados else None
def criar_conta(agencia, numero_conta, usuarios):
    cpf = input("INFORME O CPF DO USUÁRIO: ")
    usuario = filtrar_usuario(cpf, usuarios)

    if usuario:
        print("\nCONTRA CRIADA COM SUCESSO!")
        return {"agencia": agencia, "numero_conta": numero_conta, "usuario": usuario}

    print("\nUSUÁRIO NÃO ENCONTRADO, FLUXO DE CRIAÇÃO DE CONTA ENCERRADO!")


def main():
    
    saldo = 0
    limite = 500
    extrato = ""
    numero_saques = 0
    usuarios = []
    contas = []
    LIMITE_SAQUE = 3 
    AGENCIA = "0001"

    
    while True:
    
        opcao = menu()
        
        if opcao == "d":
         
            valor = float(input("INFORME O VALOR DO DEPÓSITO: "))
        
            saldo, extrato = depositar(saldo, valor, extrato)
        elif opcao == "s":
           
            valor = float(input("INFORME O VALOR DO SAQUE: "))
        
        
            saldo, extrato = sacar(
                saldo=saldo,
                valor=valor,
                extrato=extrato,
                limite=limite,
                numero_saques=numero_saques,
                limite_saques=LIMITE_SAQUE,
            )
        elif opcao == "e":
        
            exibir_extrato(saldo, extrato=extrato)
        elif opcao == "nu":
            criar_usuario(usuarios)
        elif opcao == "nc":
            numero_conta = len(contas) + 1
            conta = criar_conta(AGENCIA, numero_conta, usuarios)

            if conta:
                contas.append(conta)
        elif opcao == "q":
        
            break
        
        else: 
        
            print("OPERAÇÃO INVÁLIDA, POR FAVOR SELECIONE A OPERAÇÃO DESEJADA.")
        
main()
