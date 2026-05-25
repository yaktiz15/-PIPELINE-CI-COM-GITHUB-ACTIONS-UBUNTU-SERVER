# -PIPELINE-CI-COM-GITHUB-ACTIONS-UBUNTU-SERVER

Objetivo

O objetivo desta atividade foi implementar um pipeline de Integração Contínua (CI) utilizando GitHub Actions para automatizar a execução de testes em uma aplicação Python. Também foi realizada a configuração de um runner auto-hospedado em um servidor Ubuntu, permitindo que os jobs do pipeline fossem executados em uma máquina própria ao invés de utilizar apenas os runners gerenciados pelo GitHub.

Ambiente utilizado
Sistema Operacional: Ubuntu Server
Controle de versão: Git
Plataforma CI/CD: GitHub Actions
Linguagem utilizada: Python
Framework de testes: Pytest
Tipo de runner:
(X) GitHub-hosted
(X) Self-hosted
Parte 1 — Preparação do Ubuntu Server

Foram executados os comandos necessários para atualização do sistema e instalação das ferramentas básicas utilizadas na atividade.

Comandos executados
# Atualizar o sistema
sudo apt update && sudo apt upgrade -y

# Instalar Python, pip, git e curl
sudo apt install -y python3 python3-pip git curl

# Verificar versões
python3 --version
git --version

# Criar diretório de trabalho
mkdir ~/devops-lab
cd ~/devops-lab

<img width="1918" height="1078" alt="Captura de tela 2026-05-25 152035" src="https://github.com/user-attachments/assets/23bae110-12d0-41f7-ab35-0e06bfdea145" />

<img width="1918" height="1078" alt="Captura de tela 2026-05-25 152109" src="https://github.com/user-attachments/assets/50adfd6d-bd59-496c-8f5d-9dd489958f99" />

<img width="1918" height="1078" alt="Captura de tela 2026-05-25 152419" src="https://github.com/user-attachments/assets/1b1fc4cb-4330-41f5-b9eb-404e4292d5eb" />


Parte 2 — Criação do Projeto Python

Foi criado um repositório contendo uma aplicação simples de calculadora em Python juntamente com testes automatizados utilizando Pytest.

Estrutura do repositório
devops-ci-activity/
│
├── calculadora.py
├── test_calculadora.py
├── requirements.txt
└── .github/
    └── workflows/
        └── ci.yml
Arquivo calculadora.py
def somar(a, b):
    return a + b

def subtrair(a, b):
    return a - b

def multiplicar(a, b):
    return a * b

def dividir(a, b):
    if b == 0:
        raise ValueError("Não é possível dividir por zero")
    return a / b
Arquivo test_calculadora.py
import pytest
from calculadora import somar, subtrair, multiplicar, dividir

def test_somar():
    assert somar(2, 3) == 5
    assert somar(-1, 1) == 0

def test_subtrair():
    assert subtrair(5, 3) == 2
    assert subtrair(0, 5) == -5

def test_multiplicar():
    assert multiplicar(4, 3) == 12
    assert multiplicar(-2, 5) == -10

def test_dividir():
    assert dividir(10, 2) == 5
    assert dividir(9, 3) == 3

def test_dividir_por_zero():
    with pytest.raises(ValueError):
        dividir(5, 0)
Arquivo requirements.txt
pytest==7.4.0
Evidência 2 — Estrutura do repositório


Parte 3 — Configuração do Pipeline CI

Foi criado um workflow no GitHub Actions responsável por:

realizar checkout do código;
configurar ambiente Python;
instalar dependências;
executar testes automatizados.
Arquivo .github/workflows/ci.yml
name: CI Pipeline

on:
  push:
    branches: [ main, master ]
  pull_request:
    branches: [ main, master ]

jobs:
  test:
    runs-on: ubuntu-latest

    steps:
    - name: Checkout do código
      uses: actions/checkout@v3

    - name: Configurar Python
      uses: actions/setup-python@v4
      with:
        python-version: '3.10'

    - name: Instalar dependências
      run: |
        python -m pip install --upgrade pip
        pip install -r requirements.txt

    - name: Executar testes com pytest
      run: |
        pytest test_calculadora.py -v --tb=short
Comandos utilizados para envio ao GitHub
git add .
git commit -m "Adiciona código da calculadora e pipeline CI"
git push origin main
Evidência 3 — Pipeline executado

Inserir print do GitHub Actions mostrando:

workflow “CI Pipeline”;
etapas concluídas com sucesso;
status verde (✅).
Parte 4 — Configuração do Runner Self-hosted

Foi configurado um runner auto-hospedado no Ubuntu Server para executar os jobs diretamente no servidor do aluno.

Etapas executadas

No repositório GitHub foi acessado:

Settings → Actions → Runners → New self-hosted runner

Foi selecionado:

Linux → x64

Comandos executados no Ubuntu
mkdir actions-runner && cd actions-runner

curl -o actions-runner-linux-x64.tar.gz -L \
https://github.com/actions/runner/releases/download/v2.311.0/actions-runner-linux-x64-2.311.0.tar.gz

tar xzf ./actions-runner-linux-x64.tar.gz

./config.sh --url https://github.com/SEU-USUARIO/devops-ci-activity --token TOKEN

./run.sh

<img width="1918" height="1078" alt="Captura de tela 2026-05-25 155248" src="https://github.com/user-attachments/assets/60abe583-c74c-424b-bc1e-56f20b56565f" />



Alteração no pipeline

A linha:

runs-on: ubuntu-latest

foi alterada para:

runs-on: self-hosted

Após o commit e push, o workflow passou a executar diretamente no servidor Ubuntu configurado como runner.


Durante a atividade, algumas dificuldades foram encontradas:

configuração inicial do runner self-hosted;
permissões de execução dos scripts do GitHub Actions;
sincronização correta entre repositório local e remoto;
entendimento da estrutura YAML do workflow.

Os problemas foram solucionados utilizando documentação oficial do GitHub Actions e revisão dos comandos executados no Ubuntu.

Aprendizados

Com esta atividade foi possível aprender:

conceitos básicos de Integração Contínua (CI);
automação de testes utilizando GitHub Actions;
criação de workflows em YAML;
utilização do Pytest para testes automatizados;
configuração de runners auto-hospedados;
integração entre GitHub e servidores Linux Ubuntu.


Conclusão

A atividade permitiu implementar com sucesso um pipeline de Integração Contínua utilizando GitHub Actions e Python. Além disso, foi possível configurar um runner self-hosted em um servidor Ubuntu, possibilitando maior controle sobre o ambiente de execução dos pipelines. O projeto demonstrou na prática conceitos fundamentais de DevOps, automação e integração contínua.
