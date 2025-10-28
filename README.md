# 🖨️ Sistema de Controle de Reimpressões

![Python](https://img.shields.io/badge/Python-3.8%2B-blue)
![MySQL](https://img.shields.io/badge/MySQL-Database-orange)
![XAMPP](https://img.shields.io/badge/XAMPP-Server-green)
![Status](https://img.shields.io/badge/Status-Complete-brightgreen)

## 📋 Sobre o Projeto

Sistema profissional desenvolvido em Python com MySQL para controle e gerenciamento de solicitações de reimpressões em ambiente corporativo. Substitui planilhas manuais por um fluxo automatizado com cadastro inteligente, busca por sequência, prevenção de duplicidades e relatórios/estatísticas.

Caso de uso real: controle de reimpressões em ambiente corporativo — ideal para equipes que precisam rastrear e auditar solicitações de impressão e reimpressão.

## ⚡ Funcionalidades Principais

- ✅ Cadastro automático de solicitações de reimpressão  
- ✅ Preenchimento inteligente de dados por chapa  
- ✅ Busca avançada por sequência (completa ou parcial)  
- ✅ Validação robusta de dados e prevenção de duplicidades  
- ✅ Backup automático do banco de dados  
- ✅ Estatísticas em tempo real (totais, por impressora, por colaborador)  
- ✅ Interface em português e fluxo simples de uso  
- ✅ Configuração mínima com XAMPP

## 🛠️ Tecnologias Utilizadas

| Tecnologia | Versão | Finalidade |
|------------|:------:|------------|
| Python     | 3.8+   | Lógica da aplicação |
| MySQL      | 8.0+   | Banco de dados       |
| XAMPP      | 8.2.12+| Servidor local (MySQL) |
| mysql-connector-python | 8.0+ | Conexão com o BD |

## 🚀 Começando Rapidamente

### Pré-requisitos
- Python 3.8 ou superior
- XAMPP (MySQL) 8.2.12 ou superior

### Instalação rápida (5 minutos)

```bash
# 1. Instale Python (marque "Add to PATH")
#    https://www.python.org/downloads/
# 2. Instale XAMPP
#    https://www.apachefriends.org/pt_br/index.html
# 3. Instale dependência:
pip install mysql-connector-python

# 4. Baixe/extraia o projeto e rode:
python sistema_reimpressao.py
```

### Configuração do XAMPP
1. Abra o painel do XAMPP.  
2. Clique em "Start" no MySQL.  
3. Mantenha o MySQL executando enquanto usa o sistema.

## 📁 Estrutura do Projeto

```
sistema-reimpressoes/
│
├── backups/                 # Backups automáticos do BD
├── sistema_reimpressao.py   # Código fonte principal
├── README.md                # Documentação (este arquivo)
├── requirements.txt         # Dependências do projeto
└── LICENSE                  # Licença MIT
```

## 💾 Esquema do Banco de Dados

Exemplo da tabela principal usada pelo sistema:

```sql
CREATE TABLE reimpressoes (
    id INT AUTO_INCREMENT PRIMARY KEY,
    chapa VARCHAR(20) NOT NULL,
    nome VARCHAR(100) NOT NULL,
    horario TIME NOT NULL,
    data DATE NOT NULL,
    impressora VARCHAR(100) NOT NULL,
    sequencia VARCHAR(50),
    ramal VARCHAR(20),
    data_registro TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

## 🎮 Guia de Uso

Ao iniciar o sistema via console, é exibido o menu principal:

```
🖨️ SISTEMA DE CONTROLE DE REIMPRESSÕES
==================================================
1 - 📄 Nova solicitação de reimpressão
2 - 📋 Listar todas as solicitações
3 - 🔍 Buscar por SEQUÊNCIA
4 - 📊 Estatísticas
5 - 💾 Fazer Backup
6 - 🚪 Sair
==================================================
Escolha uma opção:
```

Fluxo de cadastro (exemplo):

```
📄 NOVA SOLICITAÇÃO DE REIMPRESSÃO
==================================================
Data automática: 2024-01-15
Hora automática: 14:30:25
--------------------------------------------------
Chapa: 12345
Nome encontrado automaticamente: João Silva
Usar este nome? (S/N): S

Impressora: HP LaserJet MFP
Sequência: ABC123
Ramal: 2100

Confirmar e INSERIR no banco de dados? (S/N): S

✅ Solicitação registrada com sucesso! ID: 15
```

Busca por sequência (parcial ou completa):

```
Digite a sequência (ou parte dela): ABC
Resultados encontrados: 3
ID: 15 | Data: 2024-01-15 | Chapa: 12345 | Nome: João Silva | Impressora: HP LaserJet MFP | Sequência: ABC123 | Ramal: 2100
```

## ⚙️ Regras de Validação e Técnicas

Validações principais implementadas:

- Chapa: apenas dígitos, entre 3 e 10 caracteres  
- Ramal: apenas dígitos, entre 3 e 5 caracteres  
- Prevenção de duplicidades por sequência no mesmo dia  
- Campos obrigatórios checados antes de inserir

Trechos exemplares (pseudo-código):

```python
# Validação de chapa (apenas números, 3-10 dígitos)
def validar_chapa(chapa):
    return chapa.isdigit() and 3 <= len(chapa) <= 10

# Validação de ramal (apenas números, 3-5 dígitos)
def validar_ramal(ramal):
    return ramal.isdigit() and 3 <= len(ramal) <= 5

# Verificação de duplicidade (exemplo)
def verificar_duplicidade(cursor, sequencia, data):
    sql = "SELECT COUNT(*) FROM reimpressoes WHERE sequencia = %s AND data = %s"
    cursor.execute(sql, (sequencia, data))
    count = cursor.fetchone()[0]
    return count > 0
```

Busca com LIKE (parcial):

```python
sql = "SELECT * FROM reimpressoes WHERE sequencia LIKE %s"
cursor.execute(sql, (f'%{sequencia}%',))
```

## 💾 Sistema de Backup

- Cria automaticamente a pasta backups/ se não existir.  
- Gera arquivos com timestamp: backup_reimpressoes_YYYYMMDD_HHMMSS.sql  
- Guarda estrutura/dados necessários para restauração.

Exemplo de nome de arquivo:
backup_reimpressoes_20240115_143025.sql

## 📊 Estatísticas

Relatórios e métricas exibidas no menu de estatísticas:

- Total de solicitações  
- Solicitações do dia  
- Impressoras mais utilizadas (top N)  
- Colaboradores mais ativos (por chapa/nome)

Exemplo de saída:

```
Total de solicitações: 150
Solicitações hoje: 8

Impressoras mais utilizadas:
  HP LaserJet MFP: 45
  Brother HL-3140: 32
  Epson L3150: 28

Colaboradores mais ativos:
  12345 - João Silva: 15
  67890 - Maria Santos: 12
  54321 - Pedro Oliveira: 10
```

## 🐛 Solução de Problemas (FAQ Rápido)

- MySQL não conecta → Verifique se o MySQL está iniciado no XAMPP  
- Módulo não encontrado → pip install mysql-connector-python  
- Access denied → Verifique usuário/senha do MySQL (XAMPP costuma usar root sem senha por padrão)  
- Porta ocupada → Feche outros serviços que usem a mesma porta

Logs e mensagens de diagnóstico:

```
❌ Erro ao conectar: [Código do erro]
⚠️ VERIFIQUE SE O XAMPP ESTÁ RODANDO:
1. Abra o XAMPP
2. Clique em 'Start' no MySQL
3. Execute o sistema novamente
```

## 🤝 Como Contribuir

1. Faça o Fork do repositório  
2. Crie uma branch: git checkout -b feature/nova-funcionalidade  
3. Commit suas alterações com mensagens claras  
4. Push para a branch e abra um Pull Request descrevendo as mudanças

## 📝 Licença

Este projeto está licenciado sob a Licença MIT. Veja o arquivo LICENSE para mais detalhes.

## 👨‍💻 Desenvolvedor

Henrique Eduardo da Maia Farias  
📧 hikemaia0@gmail.com

---

<div align="center">
⭐ Se este projeto foi útil, deixe uma estrela no repositório!
</div>## 🛠️ Tecnologias Utilizadas

| Tecnologia | Versão | Finalidade |
|------------|:------:|------------|
| Python     | 3.8+   | Lógica da aplicação |
| MySQL      | 8.0+   | Banco de dados       |
| XAMPP      | 8.2.12+| Servidor local (MySQL) |
| mysql-connector-python | 8.0+ | Conexão com o BD |

## 🚀 Começando Rapidamente

### Pré-requisitos
- Python 3.8 ou superior
- XAMPP (MySQL) 8.2.12 ou superior

### Instalação rápida (5 minutos)

```bash
# 1. Instale Python (marque "Add to PATH")
#    https://www.python.org/downloads/
# 2. Instale XAMPP
#    https://www.apachefriends.org/pt_br/index.html
# 3. Instale dependência:
pip install mysql-connector-python

# 4. Baixe/extraia o projeto e rode:
python sistema_reimpressao.py
```

### Configuração do XAMPP
1. Abra o painel do XAMPP.  
2. Clique em "Start" no MySQL.  
3. Mantenha o MySQL executando enquanto usa o sistema.

## 📁 Estrutura do Projeto

```
sistema-reimpressoes/
│
├── backups/                 # Backups automáticos do BD
├── sistema_reimpressao.py   # Código fonte principal
├── README.md                # Documentação (este arquivo)
├── requirements.txt         # Dependências do projeto
└── LICENSE                  # Licença MIT
```

## 💾 Esquema do Banco de Dados

Exemplo da tabela principal usada pelo sistema:

```sql
CREATE TABLE reimpressoes (
    id INT AUTO_INCREMENT PRIMARY KEY,
    chapa VARCHAR(20) NOT NULL,
    nome VARCHAR(100) NOT NULL,
    horario TIME NOT NULL,
    data DATE NOT NULL,
    impressora VARCHAR(100) NOT NULL,
    sequencia VARCHAR(50),
    ramal VARCHAR(20),
    data_registro TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

## 🎮 Guia de Uso

Ao iniciar o sistema via console, é exibido o menu principal:

```
🖨️ SISTEMA DE CONTROLE DE REIMPRESSÕES
==================================================
1 - 📄 Nova solicitação de reimpressão
2 - 📋 Listar todas as solicitações
3 - 🔍 Buscar por SEQUÊNCIA
4 - 📊 Estatísticas
5 - 💾 Fazer Backup
6 - 🚪 Sair
==================================================
Escolha uma opção:
```

Fluxo de cadastro (exemplo):

```
📄 NOVA SOLICITAÇÃO DE REIMPRESSÃO
==================================================
Data automática: 2024-01-15
Hora automática: 14:30:25
--------------------------------------------------
Chapa: 12345
Nome encontrado automaticamente: João Silva
Usar este nome? (S/N): S

Impressora: HP LaserJet MFP
Sequência: ABC123
Ramal: 2100

Confirmar e INSERIR no banco de dados? (S/N): S

✅ Solicitação registrada com sucesso! ID: 15
```

Busca por sequência (parcial ou completa):

```
Digite a sequência (ou parte dela): ABC
Resultados encontrados: 3
ID: 15 | Data: 2024-01-15 | Chapa: 12345 | Nome: João Silva | Impressora: HP LaserJet MFP | Sequência: ABC123 | Ramal: 2100
```

## ⚙️ Regras de Validação e Técnicas

Validações principais implementadas:

- Chapa: apenas dígitos, entre 3 e 10 caracteres  
- Ramal: apenas dígitos, entre 3 e 5 caracteres  
- Prevenção de duplicidades por sequência no mesmo dia  
- Campos obrigatórios checados antes de inserir

Trechos exemplares (pseudo-código):

```python
# Validação de chapa (apenas números, 3-10 dígitos)
def validar_chapa(chapa):
    return chapa.isdigit() and 3 <= len(chapa) <= 10

# Validação de ramal (apenas números, 3-5 dígitos)
def validar_ramal(ramal):
    return ramal.isdigit() and 3 <= len(ramal) <= 5

# Verificação de duplicidade (exemplo)
def verificar_duplicidade(cursor, sequencia, data):
    sql = "SELECT COUNT(*) FROM reimpressoes WHERE sequencia = %s AND data = %s"
    cursor.execute(sql, (sequencia, data))
    count = cursor.fetchone()[0]
    return count > 0
```

Busca com LIKE (parcial):

```python
sql = "SELECT * FROM reimpressoes WHERE sequencia LIKE %s"
cursor.execute(sql, (f'%{sequencia}%',))
```

## 💾 Sistema de Backup

- Cria automaticamente a pasta backups/ se não existir.  
- Gera arquivos com timestamp: backup_reimpressoes_YYYYMMDD_HHMMSS.sql  
- Guarda estrutura/dados necessários para restauração.

Exemplo de nome de arquivo:
backup_reimpressoes_20240115_143025.sql

## 📊 Estatísticas

Relatórios e métricas exibidas no menu de estatísticas:

- Total de solicitações  
- Solicitações do dia  
- Impressoras mais utilizadas (top N)  
- Colaboradores mais ativos (por chapa/nome)

Exemplo de saída:

```
Total de solicitações: 150
Solicitações hoje: 8

Impressoras mais utilizadas:
  HP LaserJet MFP: 45
  Brother HL-3140: 32
  Epson L3150: 28

Colaboradores mais ativos:
  12345 - João Silva: 15
  67890 - Maria Santos: 12
  54321 - Pedro Oliveira: 10
```

## 🐛 Solução de Problemas (FAQ Rápido)

- MySQL não conecta → Verifique se o MySQL está iniciado no XAMPP  
- Módulo não encontrado → pip install mysql-connector-python  
- Access denied → Verifique usuário/senha do MySQL (XAMPP costuma usar root sem senha por padrão)  
- Porta ocupada → Feche outros serviços que usem a mesma porta

Logs e mensagens de diagnóstico:

```
❌ Erro ao conectar: [Código do erro]
⚠️ VERIFIQUE SE O XAMPP ESTÁ RODANDO:
1. Abra o XAMPP
2. Clique em 'Start' no MySQL
3. Execute o sistema novamente
```

## 🤝 Como Contribuir

1. Faça o Fork do repositório  
2. Crie uma branch: git checkout -b feature/nova-funcionalidade  
3. Commit suas alterações com mensagens claras  
4. Push para a branch e abra um Pull Request descrevendo as mudanças

## 📝 Licença

Este projeto está licenciado sob a Licença MIT. Veja o arquivo LICENSE para mais detalhes.

## 👨‍💻 Desenvolvedor

Henrique Eduardo da Maia Farias  
📧 hikemaia0@gmail.com

---

<div align="center">
⭐ Se este projeto foi útil, deixe uma estrela no repositório!
</div>- ✅ Validação robusta de dados e prevenção de duplicidades
- ✅ Backup automático do banco de dados
- ✅ Estatísticas em tempo real
- ✅ Interface intuitiva em português
- ✅ Configuração zero com XAMPP

## 🛠️ Tecnologias Utilizadas

| Tecnologia | Versão | Finalidade |
|------------|--------:|------------|
| Python     | 3.8+   | Lógica da aplicação |
| MySQL      | 8.0+   | Banco de dados |
| XAMPP      | 8.2.12+| Servidor local (MySQL) |
| mysql-connector-python | 8.0+ | Conexão com o BD |

## 🚀 Instalação Rápida

### Pré-requisitos
- Python 3.8 ou superior
- XAMPP 8.2.12 ou superior

### Passo a Passo

1. Instale o Python
   - Download: https://www.python.org/downloads/
   - Marque a opção "Add Python to PATH"

2. Instale o XAMPP
   - Download: https://www.apachefriends.org/pt_br/index.html
   - Siga o instalador padrão

3. Clone o repositório (substitua os placeholders pelo seu repositório)
```bash
git clone https://github.com/SEU_USUARIO/SEU_REPO.git
cd SEU_REPO
```

4. Instale as dependências
```bash
pip install -r requirements.txt
# ou
pip install mysql-connector-python
```

5. Configure e inicie o XAMPP
- Abra o XAMPP
- Clique em "Start" no MySQL
- Deixe o MySQL rodando em segundo plano

6. Execute o sistema
```bash
python sistema_reimpressao.py
```

## 📁 Estrutura do Projeto

```
sistema-reimpressoes/
│
├── backups/                 # Backups automáticos
├── sistema_reimpressao.py   # Código principal
├── README.md                # Documentação
├── requirements.txt         # Dependências
└── LICENSE                  # Licença MIT
```

## 💾 Esquema do Banco de Dados

Exemplo de tabela usada pelo sistema:

```sql
CREATE TABLE reimpressoes (
    id INT AUTO_INCREMENT PRIMARY KEY,
    chapa VARCHAR(20) NOT NULL,
    nome VARCHAR(100) NOT NULL,
    horario TIME NOT NULL,
    data DATE NOT NULL,
    impressora VARCHAR(100) NOT NULL,
    sequencia VARCHAR(50),
    ramal VARCHAR(20),
    data_registro TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

## 🎮 Como Usar

Menu principal apresentado ao iniciar:

```
🖨️ SISTEMA DE CONTROLE DE REIMPRESSÕES
==================================================
1 - 📄 Nova solicitação de reimpressão
2 - 📋 Listar todas as solicitações
3 - 🔍 Buscar por SEQUÊNCIA
4 - 📊 Estatísticas
5 - 💾 Fazer Backup
6 - 🚪 Sair
==================================================
```

Fluxo do cadastro de solicitação:
1. Insira a chapa — o sistema busca o nome automaticamente
2. Confirme ou edite o nome
3. Digite a impressora
4. Informe a sequência
5. Digite o ramal
6. Confirme os dados antes de salvar

Busca por sequência:
- Suporta busca por sequência completa ou parcial
- Exibe todos os resultados encontrados com informações detalhadas

Estatísticas:
- Total de solicitações
- Solicitações do dia
- Impressoras mais utilizadas
- Colaboradores mais ativos

## ⚙️ Validação e Regras Técnicas

- Chapa: apenas números, 3–10 dígitos
- Ramal: apenas números, 3–5 dígitos
- Prevenção de duplicidades por sequência
- Campos obrigatórios validados antes de salvar

## 🗄️ Sistema de Backup

- Cria a pasta backups/ automaticamente (se não existir)
- Gera arquivos com timestamp
- Mantém estrutura completa do banco para restauração fácil

## 🐛 Solução de Problemas (FAQ Rápido)

- MySQL não conecta → Inicie MySQL no XAMPP
- Módulo não encontrado → pip install mysql-connector-python
- Acesso negado → XAMPP usa senha em branco por padrão (verifique configurações)
- Porta ocupada → Feche outros programas que usem a mesma porta do MySQL

Mensagens de confirmação comuns:
```
✅ Conectado ao XAMPP com sucesso!
✅ Banco de dados pronto!
✅ Solicitação registrada com sucesso!
```

## 📊 Exemplo de Estatísticas

```
📊 ESTATÍSTICAS DO SISTEMA
==================================================
Total de solicitações: 150
Solicitações hoje: 8

🖨️ Impressoras mais utilizadas:
  HP LaserJet MFP: 45 solicitações
  Brother HL-3140: 32 solicitações

👥 Colaboradores mais ativos:
  12345 - João Silva: 15 solicitações
  67890 - Maria Santos: 12 solicitações
```

## Contribuição

Contribuições são bem-vindas! Siga o fluxo abaixo:
1. Faça um fork do repositório
2. Crie uma branch: git checkout -b feat/minha-feature
3. Faça commits claros e atômicos
4. Abra um Pull Request descrevendo as mudanças e como testar

## Licença

Este projeto está licenciado sob a Licença MIT. Consulte o arquivo LICENSE para detalhes.

## 👨‍💻 Desenvolvedor

Henrique Eduardo da Maia Farias  
📧 hikemaia0@gmail.com

---

<div align="center">
⭐ Se este projeto foi útil, deixe uma estrela no repositório!
</div>   - Marque a opção "Add Python to PATH"
