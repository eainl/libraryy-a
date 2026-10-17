# libraryy-a
#A project for my programming course (?)

# ATIVIDADE 8 - SISTEMA DE GERENCIAMENTO DE LIVROS !!

import json

# INFORMAÇÕES PARA O COMPUTADOR <

livros = []


# SALVANDO EM JSON <

def salvar_livros():
    with open("livros.json", "w", encoding="utf-8") as f:
        json.dump(livros, f, ensure_ascii=False, indent=4)


def carregar_livros():
    global livros

    try:
        with open("livros.json", "r", encoding="utf-8") as f:
            livros = json.load(f)

        # Adiciona disponibilidade aos livros antigos
        for livro in livros:
            if "disponivel" not in livro:
                livro["disponivel"] = "sim"

    except FileNotFoundError:
        livros = []


# MECANISMOS <

def cadastrar():
    titulo = input("Digite o título do livro: ")
    autor = input("Digite o autor do livro: ")
    ano = input("Digite o ano de publicação: ")

    livro = {
        "titulo": titulo,
        "autor": autor,
        "ano": ano,
        "disponivel": "sim"
    }

    livros.append(livro)
    salvar_livros()

    print("Livro cadastrado com sucesso!")


def listar():
    if not livros:
        print("Nenhum livro cadastrado.")

    else:
        for livro in livros:
            print("--------------------")
            print("Título:", livro["titulo"])
            print("Autor:", livro["autor"])
            print("Ano:", livro["ano"])
            print("Disponível:", livro["disponivel"])


def buscar():
    busca = input("Digite o título ou autor do livro: ")
    encontrado = False

    for livro in livros:
        if (busca.lower() in livro["titulo"].lower()
            or busca.lower() in livro["autor"].lower()):

            print("Título:", livro["titulo"])
            print("Autor:", livro["autor"])
            print("Ano:", livro["ano"])
            print("Disponível:", livro["disponivel"])

            encontrado = True

    if encontrado == False:
        print("Livro não encontrado.")


def emprestar():
    titulo = input("Digite o título do livro que deseja emprestar: ")

    for livro in livros:
        if livro["titulo"].lower() == titulo.lower():

            if livro["disponivel"] == "sim":
                livro["disponivel"] = "não"
                salvar_livros()
                print("Livro emprestado com sucesso!")

            else:
                print("Este livro já está emprestado!")

            return

    print("Livro não encontrado.")


def devolver():
    titulo = input("Digite o título do livro que deseja devolver: ")

    for livro in livros:
        if livro["titulo"].lower() == titulo.lower():

            if livro["disponivel"] == "não":
                livro["disponivel"] = "sim"
                salvar_livros()
                print("Livro devolvido com sucesso!")

            else:
                print("Este livro já está disponível!")

            return

    print("Livro não encontrado.")


def organizar():
    if len(livros) == 0:
        print("Nenhum livro cadastrado.")

    else:
        livros.sort(key=lambda x: x["titulo"].lower())
        salvar_livros()

        print("Livros organizados por título:")

        for livro in livros:
            print(livro["titulo"], "-", livro["autor"])


def contar():
    total = len(livros)
    disponiveis = 0
    emprestados = 0

    for livro in livros:
        if livro["disponivel"] == "sim":
            disponiveis += 1
        else:
            emprestados += 1

    print("Total de livros:", total)
    print("Livros disponíveis:", disponiveis)
    print("Livros emprestados:", emprestados)


# MENU PRINCIPAL <

carregar_livros()

escolha = ""

print("Boas-vindas ao sistema de gerenciamento de livros!")

while escolha != "6":
    print("\n===== MENU PRINCIPAL =====")
    print("1. Cadastrar livro")
    print("2. Listar livros")
    print("3. Buscar livro")
    print("4. Devolver livro")
    print("5. Emprestar livro")
    print("6. Sair")
    print("7. Organizar livros")
    print("8. Contar livros")

    escolha = input("Escolha uma opção: ")

    if escolha == "1":
        cadastrar()

    elif escolha == "2":
        listar()

    elif escolha == "3":
        buscar()

    elif escolha == "4":
        devolver()

    elif escolha == "5":
        emprestar()

    elif escolha == "6":
        print("Até logo, volte sempre!")

    elif escolha == "7":
        organizar()

    elif escolha == "8":
        contar()

    else:
        print("Opção inválida. Tente novamente.")
