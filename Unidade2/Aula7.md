{
  "cells": [
    {
      "cell_type": "markdown",
      "metadata": {
        "id": "view-in-github",
        "colab_type": "text"
      },
      "source": [
        "<a href=\"https://colab.research.google.com/github/Italocss/Engenharia-de-Prompt-e-Aplica-es-em-Ia/blob/main/Aula7\" target=\"_parent\"><img src=\"https://colab.research.google.com/assets/colab-badge.svg\" alt=\"Open In Colab\"/></a>"
      ]
    },
    {
      "cell_type": "markdown",
      "metadata": {
        "id": "300cd916"
      },
      "source": [
        "# Calculadora Simples em Python"
      ]
    },
    {
      "cell_type": "code",
      "metadata": {
        "id": "47ecf641"
      },
      "source": [
        "def adicionar(x, y):\n",
        "    return x + y\n",
        "\n",
        "def subtrair(x, y):\n",
        "    return x - y\n",
        "\n",
        "def multiplicar(x, y):\n",
        "    return x * y\n",
        "\n",
        "def dividir(x, y):\n",
        "    if y == 0:\n",
        "        return \"Erro! Divisão por zero!\"\n",
        "    return x / y\n"
      ],
      "execution_count": 4,
      "outputs": []
    },
    {
      "cell_type": "markdown",
      "metadata": {
        "id": "babb7a93"
      },
      "source": [
        "## Demonstração da Calculadora"
      ]
    },
    {
      "cell_type": "code",
      "metadata": {
        "colab": {
          "base_uri": "https://localhost:8080/"
        },
        "id": "ea44fcda",
        "outputId": "36d95b89-2898-4ee7-e994-395987af7000"
      },
      "source": [
        "num1 = 10\n",
        "num2 = 5\n",
        "\n",
        "operacoes = {\n",
        "    '+': adicionar,\n",
        "    '-': subtrair,\n",
        "    '*': multiplicar,\n",
        "    '/': dividir\n",
        "}\n",
        "\n",
        "for simbolo, funcao in operacoes.items():\n",
        "    resultado = funcao(num1, num2)\n",
        "    print(f\"{num1} {simbolo} {num2} = {resultado}\")\n",
        "\n",
        "print(\"\\n--- Teste de Divisão por Zero ---\")\n",
        "num3 = 10\n",
        "num4 = 0\n",
        "print(f\"{num3} / {num4} = {dividir(num3, num4)}\")"
      ],
      "execution_count": 5,
      "outputs": [
        {
          "output_type": "stream",
          "name": "stdout",
          "text": [
            "10 + 5 = 15\n",
            "10 - 5 = 5\n",
            "10 * 5 = 50\n",
            "10 / 5 = 2.0\n",
            "\n",
            "--- Teste de Divisão por Zero ---\n",
            "10 / 0 = Erro! Divisão por zero!\n"
          ]
        }
      ]
    },
    {
      "cell_type": "markdown",
      "metadata": {
        "id": "1d165a73"
      },
      "source": [
        "# Validadores de CEP e CPF e Requisições REST\n",
        "\n",
        "Nesta seção, criaremos funções em Python para validar Códigos de Endereçamento Postal (CEP) e Cadastro de Pessoas Físicas (CPF), além de demonstrar como fazer requisições HTTP a APIs externas e manipular dados JSON. Também incluiremos um código boilerplate genérico para interações RESTful."
      ]
    },
    {
      "cell_type": "markdown",
      "metadata": {
        "id": "95820774"
      },
      "source": [
        "## Validação de CEP\n",
        "\n",
        "O Código de Endereçamento Postal (CEP) é um sistema de códigos postais utilizado no Brasil. Vamos primeiro validar o formato do CEP e depois consultar uma API pública para obter informações de endereço."
      ]
    },
    {
      "cell_type": "code",
      "metadata": {
        "id": "67589110"
      },
      "source": [
        "import re\n",
        "import requests\n",
        "\n",
        "def validar_cep_formato(cep):\n",
        "    \"\"\"Valida o formato de um CEP brasileiro (apenas dígitos ou com hífen).\"\"\"\n",
        "    # Remove o hífen se presente para validar o formato de 8 dígitos\n",
        "    cep_limpo = str(cep).replace('-', '')\n",
        "    if re.fullmatch(r'^\\d{8}$', cep_limpo):\n",
        "        return True\n",
        "    return False\n",
        "\n",
        "def consultar_cep_api(cep):\n",
        "    \"\"\"Consulta uma API de CEP (ViaCEP) para obter dados de endereço.\n",
        "\n",
        "    Args:\n",
        "        cep (str): O CEP a ser consultado.\n",
        "\n",
        "    Returns:\n",
        "        dict: Um dicionário com os dados do endereço, ou None se houver erro.\n",
        "    \"\"\"\n",
        "    if not validar_cep_formato(cep):\n",
        "        print(f\"CEP inválido: {cep}. Formato esperado: XXXXXXXX ou XX.XXX-XXX\")\n",
        "        return None\n",
        "\n",
        "    cep_limpo = str(cep).replace('-', '')\n",
        "    url = f\"https://viacep.com.br/ws/{cep_limpo}/json/\"\n",
        "\n",
        "    try:\n",
        "        response = requests.get(url)\n",
        "        response.raise_for_status()  # Lança uma exceção para códigos de status HTTP de erro (4xx ou 5xx)\n",
        "        data = response.json()\n",
        "\n",
        "        if 'erro' in data and data['erro']:\n",
        "            print(f\"CEP {cep} não encontrado na API.\")\n",
        "            return None\n",
        "\n",
        "        return data\n",
        "    except requests.exceptions.RequestException as e:\n",
        "        print(f\"Erro ao fazer a requisição HTTP para o CEP {cep}: {e}\")\n",
        "        return None\n"
      ],
      "execution_count": 6,
      "outputs": []
    },
    {
      "cell_type": "markdown",
      "metadata": {
        "id": "1a9c7398"
      },
      "source": [
        "## Validação de CPF\n",
        "\n",
        "O Cadastro de Pessoas Físicas (CPF) é um registro da Receita Federal do Brasil. A validação de um CPF envolve um algoritmo matemático que verifica os dois dígitos verificadores. Não faremos requisições a APIs para CPF devido a questões de privacidade e acesso a dados sensíveis, mas a validação do algoritmo local é robusta."
      ]
    },
    {
      "cell_type": "code",
      "metadata": {
        "id": "bde6cad1"
      },
      "source": [
        "def validar_cpf(cpf):\n",
        "    \"\"\"Valida um CPF brasileiro usando seu algoritmo de dígitos verificadores.\n",
        "\n",
        "    Args:\n",
        "        cpf (str): O CPF a ser validado (apenas dígitos).\n",
        "\n",
        "    Returns:\n",
        "        bool: True se o CPF for válido, False caso contrário.\n",
        "    \"\"\"\n",
        "    cpf = str(cpf).replace('.', '').replace('-', '') # Remove pontos e traços\n",
        "\n",
        "    # Verifica se o CPF tem 11 dígitos e se não é uma sequência de dígitos iguais\n",
        "    if not cpf.isdigit() or len(cpf) != 11 or len(set(cpf)) == 1:\n",
        "        return False\n",
        "\n",
        "    # Calcula o primeiro dígito verificador\n",
        "    soma = 0\n",
        "    for i in range(9):\n",
        "        soma += int(cpf[i]) * (10 - i)\n",
        "    resto = 11 - (soma % 11)\n",
        "    digito1 = 0 if resto > 9 else resto\n",
        "\n",
        "    # Verifica o primeiro dígito\n",
        "    if not (int(cpf[9]) == digito1):\n",
        "        return False\n",
        "\n",
        "    # Calcula o segundo dígito verificador\n",
        "    soma = 0\n",
        "    for i in range(10):\n",
        "        soma += int(cpf[i]) * (11 - i)\n",
        "    resto = 11 - (soma % 11)\n",
        "    digito2 = 0 if resto > 9 else resto\n",
        "\n",
        "    # Verifica o segundo dígito\n",
        "    if not (int(cpf[10]) == digito2):\n",
        "        return False\n",
        "\n",
        "    return True\n"
      ],
      "execution_count": 7,
      "outputs": []
    },
    {
      "cell_type": "markdown",
      "metadata": {
        "id": "da32d0f1"
      },
      "source": [
        "## Código Boilerplate REST Genérico\n",
        "\n",
        "Para interagir com APIs REST de forma mais estruturada, podemos criar uma classe ou funções genéricas que encapsulam a lógica de requisições HTTP e manipulação de JSON. Isso é útil para reutilização de código e para manter as interações com APIs consistentes."
      ]
    },
    {
      "cell_type": "code",
      "metadata": {
        "id": "edcf2fa9"
      },
      "source": [
        "import requests\n",
        "import json\n",
        "\n",
        "class RestClient:\n",
        "    \"\"\"Um cliente REST genérico para fazer requisições HTTP e manipular JSON.\"\"\"\n",
        "\n",
        "    def __init__(self, base_url, headers=None):\n",
        "        self.base_url = base_url\n",
        "        self.headers = headers if headers is not None else {'Content-Type': 'application/json'}\n",
        "\n",
        "    def _request(self, method, endpoint, data=None, params=None):\n",
        "        url = f\"{self.base_url}/{endpoint}\"\n",
        "        try:\n",
        "            if method == 'GET':\n",
        "                response = requests.get(url, headers=self.headers, params=params)\n",
        "            elif method == 'POST':\n",
        "                response = requests.post(url, headers=self.headers, json=data, params=params)\n",
        "            elif method == 'PUT':\n",
        "                response = requests.put(url, headers=self.headers, json=data, params=params)\n",
        "            elif method == 'DELETE':\n",
        "                response = requests.delete(url, headers=self.headers, params=params)\n",
        "            else:\n",
        "                raise ValueError(f\"Método HTTP não suportado: {method}\")\n",
        "\n",
        "            response.raise_for_status()  # Levanta HTTPError para 4xx/5xx\n",
        "\n",
        "            # Tenta retornar JSON, se não for possível, retorna o texto\n",
        "            try:\n",
        "                return response.json()\n",
        "            except json.JSONDecodeError:\n",
        "                return response.text\n",
        "\n",
        "        except requests.exceptions.HTTPError as http_err:\n",
        "            print(f\"Erro HTTP ao fazer requisição {method} para {url}: {http_err}\")\n",
        "            print(f\"Resposta da API (erro): {response.text}\")\n",
        "            return None\n",
        "        except requests.exceptions.ConnectionError as conn_err:\n",
        "            print(f\"Erro de conexão ao fazer requisição {method} para {url}: {conn_err}\")\n",
        "            return None\n",
        "        except requests.exceptions.Timeout as timeout_err:\n",
        "            print(f\"Timeout ao fazer requisição {method} para {url}: {timeout_err}\")\n",
        "            return None\n",
        "        except requests.exceptions.RequestException as req_err:\n",
        "            print(f\"Erro inesperado ao fazer requisição {method} para {url}: {req_err}\")\n",
        "            return None\n",
        "\n",
        "    def get(self, endpoint, params=None):\n",
        "        return self._request('GET', endpoint, params=params)\n",
        "\n",
        "    def post(self, endpoint, data=None):\n",
        "        return self._request('POST', endpoint, data=data)\n",
        "\n",
        "    def put(self, endpoint, data=None):\n",
        "        return self._request('PUT', endpoint, data=data)\n",
        "\n",
        "    def delete(self, endpoint, params=None):\n",
        "        return self._request('DELETE', endpoint, params=params)\n"
      ],
      "execution_count": 8,
      "outputs": []
    },
    {
      "cell_type": "markdown",
      "metadata": {
        "id": "403bb229"
      },
      "source": [
        "## Demonstrações"
      ]
    },
    {
      "cell_type": "code",
      "metadata": {
        "colab": {
          "base_uri": "https://localhost:8080/"
        },
        "id": "a6c7e1c6",
        "outputId": "e2f02df4-9686-4be0-fcc6-41cc113937de"
      },
      "source": [
        "# --- Demonstração de Validação de CEP ---\n",
        "print(\"--- Validação de CEP ---\")\n",
        "\n",
        "cep_valido = \"01001-000\"\n",
        "cep_invalido_formato = \"12345-67\"\n",
        "cep_inexistente = \"99999-999\"\n",
        "\n",
        "print(f\"CEP {cep_valido} (formato válido): {validar_cep_formato(cep_valido)}\")\n",
        "print(f\"CEP {cep_invalido_formato} (formato inválido): {validar_cep_formato(cep_invalido_formato)}\")\n",
        "\n",
        "dados_cep_valido = consultar_cep_api(cep_valido)\n",
        "if dados_cep_valido:\n",
        "    print(f\"Dados do CEP {cep_valido}:\")\n",
        "    print(f\"  Logradouro: {dados_cep_valido.get('logradouro')}\")\n",
        "    print(f\"  Bairro: {dados_cep_valido.get('bairro')}\")\n",
        "    print(f\"  Localidade: {dados_cep_valido.get('localidade')}\")\n",
        "    print(f\"  UF: {dados_cep_valido.get('uf')}\")\n",
        "\n",
        "dados_cep_inexistente = consultar_cep_api(cep_inexistente)\n",
        "if dados_cep_inexistente is None:\n",
        "    print(f\"Consulta para CEP {cep_inexistente} retornou erro ou não encontrado (como esperado).\")\n",
        "\n",
        "print(\"\\n--- Validação de CPF ---\")\n",
        "# --- Demonstração de Validação de CPF ---\n",
        "cpf_valido = \"123.456.789-00\" # Exemplo de CPF válido (se seguir a lógica do algoritmo)\n",
        "cpf_invalido_digito = \"123.456.789-01\" # CPF com dígito errado\n",
        "cpf_sequencia = \"111.111.111-11\" # Sequência de dígitos iguais\n",
        "\n",
        "print(f\"CPF {cpf_valido}: {validar_cpf(cpf_valido)}\")\n",
        "print(f\"CPF {cpf_invalido_digito}: {validar_cpf(cpf_invalido_digito)}\")\n",
        "print(f\"CPF {cpf_sequencia}: {validar_cpf(cpf_sequencia)}\")\n",
        "\n",
        "print(\"\\n--- Demonstração do Cliente REST Genérico ---\")\n",
        "# --- Demonstração do Cliente REST Genérico ---\n",
        "# Usando a API ViaCEP como exemplo para GET\n",
        "api_cep_base = \"https://viacep.com.br/ws\"\n",
        "client_cep = RestClient(api_cep_base)\n",
        "\n",
        "# Exemplo de GET para CEP\n",
        "print(f\"Consulta de CEP {cep_valido} usando RestClient:\")\n",
        "response_cep_client = client_cep.get(f\"{cep_valido.replace('-','')}/json/\")\n",
        "if response_cep_client:\n",
        "    print(f\"  Localidade: {response_cep_client.get('localidade')}\")\n",
        "\n",
        "# Exemplo de um POST (usando uma API de teste fake, pois ViaCEP é só GET)\n",
        "# Esta API é para demonstração de POST/PUT/DELETE, não tente enviar dados reais de usuário.\n",
        "fake_api_base = \"https://jsonplaceholder.typicode.com\"\n",
        "client_fake = RestClient(fake_api_base)\n",
        "\n",
        "print(\"\\nExemplo de POST para uma API fake:\")\n",
        "new_post_data = {\n",
        "    \"title\": \"foo\",\n",
        "    \"body\": \"bar\",\n",
        "    \"userId\": 1\n",
        "}\n",
        "post_response = client_fake.post(\"posts\", data=new_post_data)\n",
        "if post_response:\n",
        "    print(\"  Post criado com sucesso:\")\n",
        "    print(f\"    ID: {post_response.get('id')}\")\n",
        "    print(f\"    Title: {post_response.get('title')}\")\n",
        "\n",
        "print(\"\\nExemplo de GET de um item (do POST anterior, se fosse real):\")\n",
        "get_single_post = client_fake.get(\"posts/1\")\n",
        "if get_single_post:\n",
        "    print(\"  Primeiro post (GET):\")\n",
        "    print(f\"    Title: {get_single_post.get('title')}\")\n",
        "\n"
      ],
      "execution_count": 9,
      "outputs": [
        {
          "output_type": "stream",
          "name": "stdout",
          "text": [
            "--- Validação de CEP ---\n",
            "CEP 01001-000 (formato válido): True\n",
            "CEP 12345-67 (formato inválido): False\n",
            "Dados do CEP 01001-000:\n",
            "  Logradouro: Praça da Sé\n",
            "  Bairro: Sé\n",
            "  Localidade: São Paulo\n",
            "  UF: SP\n",
            "CEP 99999-999 não encontrado na API.\n",
            "Consulta para CEP 99999-999 retornou erro ou não encontrado (como esperado).\n",
            "\n",
            "--- Validação de CPF ---\n",
            "CPF 123.456.789-00: False\n",
            "CPF 123.456.789-01: False\n",
            "CPF 111.111.111-11: False\n",
            "\n",
            "--- Demonstração do Cliente REST Genérico ---\n",
            "Consulta de CEP 01001-000 usando RestClient:\n",
            "  Localidade: São Paulo\n",
            "\n",
            "Exemplo de POST para uma API fake:\n",
            "  Post criado com sucesso:\n",
            "    ID: 101\n",
            "    Title: foo\n",
            "\n",
            "Exemplo de GET de um item (do POST anterior, se fosse real):\n",
            "  Primeiro post (GET):\n",
            "    Title: sunt aut facere repellat provident occaecati excepturi optio reprehenderit\n"
          ]
        }
      ]
    }
  ],
  "metadata": {
    "colab": {
      "name": "Conheça o Colab",
      "toc_visible": true,
      "provenance": [],
      "include_colab_link": true
    },
    "kernelspec": {
      "display_name": "Python 3",
      "name": "python3"
    }
  },
  "nbformat": 4,
  "nbformat_minor": 0
}
