
# 📘 Documentação do Projeto de Automação de E-mails – Interisk

## 📌 O que é Python?
Python é uma linguagem de programação de alto nível, conhecida por sua simplicidade e legibilidade. É amplamente utilizada para automações, análise de dados, desenvolvimento web, inteligência artificial e muito mais.

> No nosso projeto, Python é a base para automação de login, download de planilhas e envio de e-mails inteligentes conforme regras de negócio.

---

## 1. O que é o Python?
O **Python** é uma linguagem de programação considerada "fácil" de aprender por sua simples sintaxes e por ser uma *linguagem de auto nivel(mais proxima do ser humano)*, *linguagens de baixo nivel* como C, Java... são de baixo nivel mais proxima do computador. python é muito usada para automação de tarefas, análise de dados, webscraping, raspagem de dados, inteligência artificial e muito mais.

### ✅ Vantagens do Python:
- Simples e intuitivo
- Muito bem documentado( https://docs.python.org/3.15/ )
- Possui milhares de bibliotecas prontas para uso

---

## 2. O que é uma biblioteca Python?
Bibliotecas são pacotes prontos com funções e recursos específicos. Por exemplo:
- `pandas`: manipula planilhas e tabelas de dados
- `smtplib`: envia e-mails
- `email.message`: constrói mensagens de e-mail
- `openpyxl`: trabalha com arquivos `.xlsx`

---




## ⚙️ Objetivo da Automação

Automatizar o processo de:
1. Login no sistema Interisk.
2. Download de planilhas com apontamentos de auditoria.
3. Envio automático de e-mails com:
   - Alerta de **planos de ação vencidos**.
   - Lembrete para **planos com vencimento próximo**.
4. Evitar reenvio duplicado com **registro em Excel**.

---

### utilize boas praticas de códigos

- Nunca construa sua estrutura tudo em 1 codigo, separe por partes e rode tudo no `main.py`

## 🗂️ Estrutura de Pastas

```
automacao_interisk/
│
├── main.py                        # Ponto de entrada da automação
│
├── .env                           # Variáveis de ambiente sensíveis
│
├── planilha/                      # Planilhas exportadas e registros
│   ├── export.xlsx                # Planilha baixada do Interisk
│   └── registros_envios.xlsx     # Histórico de e-mails enviados
│
├── mapeamentoEmailSegmento/      
│   └── emails_segmentos2.xlsx    # Mapeamento dos e-mails por segmento
│
├── ArquivosPadrao/               
│   └── LogoCrediSISBranco.png    # Logo usada nos e-mails
│
├── login/
│   └── login.py                  # Script de login automatizado
│
├── download/
│   └── downloadPlanilha.py      # Script para baixar e renomear a planilha
│
└── envioEmails/
    ├── emails_planosVencidos.py # Envio de e-mails de atrasados
    └── vencimento_proximo.py    # Envio de e-mails de lembrete
```

---

## O que é uma Variável de Ambiente?

Uma variável de ambiente é um dado sensível (como senhas) que não deve ser exposto no código. Guardamos isso no arquivo `.env`.

### Exemplo de `.env` seguro:

Configure um arquivo `.env` com:

```dotenv
CAMINHO_PLANILHA=C:/Users/SeuUsuario/Desktop/automacao_interisk/planilha
CAMINHO_MAPEAMENTO=C:/Users/SeuUsuario/Desktop/automacao_interisk/mapeamentoEmailSegmento
CAMINHO_PADRAO=C:/Users/SeuUsuario/Desktop/automacao_interisk/ArquivosPadrao

SMTP_USER=seu_email@gmail.com
SMTP_PASS=sua_senha_de_aplicativo
```

Nunca envie esse arquivo para o GitHub! Coloque ele no `.gitignore`.


### 🔐 Como gerar uma senha de aplicativo no Gmail:

#### Se tiver ativar o 2FA:
- Acesse:[app PassWord](https://myaccount.google.com/apppasswords)

- Se estiver com 2FA ativado, você verá uma tela para criar Senhas de app (escolha "Mail" e "Outro → Python").
- Gere uma senha de 16 caracteres para “Mail” com nome personalizado (ex: Automação Python)
- Use essa senha no campo `SMTP_SENHA` do `.env

#### Se não tiver ativado o 2FA:

1. Vá em [Google Security](https://myaccount.google.com/security)
2. Ative a verificação em duas etapas
3. Após isso, acesse a opção **Senhas de app**
4. Gere uma senha de 16 caracteres para “Mail” com nome personalizado (ex: Automação Python)
5. Use essa senha no campo `SMTP_SENHA` do `.env`

---

## O que são funções em Python?

Funções são blocos de código reutilizáveis. Por exemplo:
#### Python
```python
def saudacao(nome):
    print(f"Olá, {nome}!")

saudacao("Maria")
```

- Simples, sem declarar tipo de dado, nem ponto e vírgula

#### C
```C

#include <stdio.h>

void saudacao(char nome[]) {
    printf("Olá, %s!
", nome);
}

int main() {
    saudacao("Maria");
    return 0;
}
```

- Tipagem obrigatória, uso de ponto e vírgula, e função main()

#### Java
```Java

public class Saudacao {
    public static void saudacao(String nome) {
        System.out.println("Olá, " + nome + "!");
    }

    public static void main(String[] args) {
        saudacao("Maria");
    }
}
```
- Uso de classes, métodos estáticos e sintaxe mais detalhada

#### C#
```C#
using System;

class Saudacao {
    static void Saudacao(string nome) {
        Console.WriteLine($"Olá, {nome}!");
    }

    static void Main() {
        Saudacao("Maria");
    }
}

```
- Uso de classes, métodos estáticos e sintaxe mais detalhada

#### Assembly
```Assembly
section .data
    msg db 'Olá, Maria!', 0xA   ; string + quebra de linha
    len equ $ - msg             ; comprimento da string

section .text
    global _start

_start:
    ; syscall write (stdout)
    mov edx, len        ; tamanho da mensagem
    mov ecx, msg        ; endereço da string
    mov ebx, 1          ; file descriptor (stdout)
    mov eax, 4          ; syscall número 4: sys_write
    int 0x80            ; chamada de sistema

    ; syscall exit
    mov eax, 1          ; syscall número 1: sys_exit
    xor ebx, ebx        ; código de saída 0
    int 0x80

```
### 🧾 O que isso faz:
- Define uma mensagem `"Olá, Maria!"` na seção de dados.

- Usa instruções para chamar o serviço do sistema operacional e escreve no terminal.

- Depois, termina o programa com `sys_exit`.

⚠️ Assembly é muito próximo do hardware e exige conhecimento de registradores (eax, ebx, etc.) e interrupções do sistema (int 0x80).


### ✅ Moral da comparação:

- Python: simples, direto, ideal para automações rápidas.

- C/Java/C#: mais verbosos, exigem estrutura, mas poderosos em aplicações maiores.

- Assembly: extremamente próximo do hardware, difícil de ler e escrever.
  

## 🔑 Diferença entre parâmetro e argumento:
Para entender como uma função funciona, é importante saber o que são **parâmetros** e **argumentos**.

- **Parâmetro** é como se fosse uma caixinha que a função espera receber para funcionar. Ele é definido no momento em que a função é criada. Pense como um espaço reservado para um valor.

- **Argumento** é o valor real que você envia para a função quando a chama. É como colocar um valor real dentro da caixinha.

Exemplo para entender melhor:
```python
def saudacao(nome):  # "nome" é o parâmetro
    print(f"Olá, {nome}!")

saudacao("Maria")     # "Maria" é o argumento
```
[...]

## 📌 Funções utilizadas no projeto:


## 🔧 `fazer_login(driver)`

```python
def fazer_login(driver):
    # Lógica para preencher o login no sistema Interisk usando Selenium
    ...
```

**O que faz?**  
Essa função realiza o login automático no site do sistema Interisk.  
Ela usa o navegador automatizado (Selenium) para preencher o nome de usuário e senha e clicar no botão de "Entrar".

**Por que é importante?**  
Evita que o usuário precise acessar o sistema manualmente sempre que rodar o script.

---

## 📥 `baixar_planilha(driver)`

```python
def baixar_planilha(driver):
    # Acessa a página de apontamentos e baixa a planilha export.xlsx
    ...
```

**O que faz?**  
Depois do login, o sistema navega até a página dos apontamentos e baixa uma planilha chamada `export.xlsx` com todos os dados necessários para o monitoramento.

**Por que é importante?**  
A automação depende dessas informações para saber quais planos estão vencidos ou próximos de vencer.

---

## 📁 `load_data(file_path)`

```python
def load_data(file_path):
    data = pd.read_excel(file_path)
    ...
    return data
```

**O que faz?**  
Lê a planilha baixada (`export.xlsx`) e transforma os dados em uma estrutura que o Python entende (DataFrame).

**Por que é importante?**  
Permite que o script analise prazos, datas e nomes de responsáveis.

---

## 📤 `send_email(...)`

```python
def send_email(self, from_addr, to_addrs, subject, html_content, logo_path=None, cc=None):
    # Monta e envia um e-mail com conteúdo personalizado e com logo
    ...
```

**O que faz?**  
Envia um e-mail personalizado para os responsáveis de cada plano de ação.  
Inclui o conteúdo do plano, data de vencimento, tempo em atraso e um botão com link direto ao sistema.

**Por que é importante?**  
Garante comunicação automática e efetiva com os responsáveis pelos planos.

---

## 📍 `filter_expired_actions(data)`

```python
def filter_expired_actions(data):
    # Filtra apenas os planos que estão vencidos
    ...
```

**O que faz?**  
Seleciona da planilha somente os planos que já passaram da data de vencimento.

**Por que é importante?**  
Evita enviar e-mail para planos que ainda estão dentro do prazo.

---

## 📅 `filter_data_by_deadline(data, days_ahead=60)`

```python
def filter_data_by_deadline(data, days_ahead=60):
    # Seleciona planos com vencimento nos próximos 60 dias
    ...
```

**O que faz?**  
Filtra os planos de ação que ainda estão dentro do prazo, mas que vencem nos próximos 60 dias.

**Por que é importante?**  
Permite o envio de lembretes antecipados para evitar atrasos.

---

## 💌 `enviar_emails_vencidos(df)`

```python
def enviar_emails_vencidos(df):
    # Agrupa por segmento e envia e-mails dos planos vencidos
    ...
```

**O que faz?**  
Envia e-mails para os planos que já estão em atraso.

**Por que é importante?**  
Ajuda a manter a gestão dos prazos em dia e cobrar o retorno das áreas responsáveis.

---

## ⏳ `enviar_emails_proximos(df)`

```python
def enviar_emails_proximos(df):
    # Agrupa e envia os e-mails para planos com vencimento próximo
    ...
```

**O que faz?**  
Envia lembretes automáticos para os responsáveis de planos de ação que ainda estão dentro do prazo, mas perto do vencimento.

**Por que é importante?**  
Ajuda a prevenir que novos atrasos aconteçam.

---

## 💾 Registro de Envios

O sistema salva todos os e-mails enviados em um arquivo chamado `registros_envios.xlsx`.  
Isso impede que o mesmo plano seja notificado duas vezes.

---

## ⏱ Espera antes do envio

O sistema espera 30 segundos antes de começar os envios, garantindo que o download da planilha tenha sido finalizado.

---



---




## Como funciona o `main.py`

O `main.py` apenas chama a função principal:
```python
from modules.envio import processar_emails

if __name__ == "__main__":
    processar_emails()
```

---

## Como agendar a execução automaticamente (cron)

### Windows:
Use o **Agendador de Tarefas** para rodar `python main.py` de hora em hora.

### Linux/Mac:
Edite o `crontab`:
```bash
crontab -e
```
E adicione:
```
0 * * * * /usr/bin/python3 /caminho/para/email_automacao/main.py
```

---





## 🔄 Fluxo da Automação

1. O `main.py`:
   - Faz login com Selenium.
   - Baixa a planilha e salva como `export.xlsx`.
   - Aguarda 30 segundos.
   - Verifica novos registros ainda não enviados.
   - Envia e-mails segmentados por cooperativa.
   - Registra o envio em `registros_envios.xlsx`.

2. Os scripts de e-mail (`envioEmails/`) formatam o HTML com base em templates profissionais e enviam para os destinatários de cada segmento.

---

## ✅ Controle de Reenvio

- Toda vez que um e-mail é enviado, os dados são salvos em `registros_envios.xlsx`.
- Antes de enviar novamente, o sistema verifica se o mesmo **Apontamento N° + Segmento** já foi registrado.

---

## 🧠 O que abordar na apresentação?

- O problema: envio manual, risco de atraso.
- A solução: automação com Python e Selenium.
- Benefícios:
  - Padronização visual dos e-mails.
  - Redução de esforço humano.
  - Prevenção de reenvios.
- Como funciona (fluxograma simples).
- Demonstração ou prints do script rodando.
- Próximos passos (ex: envio em lote, log detalhado, logs por e-mail, painel de controle).


---

## Instalação do Python e Bibliotecas

### a) Instalar o Python:
Caso não tenha o Python, você provavelmente vai ter que abrir um chamado para a instalação dele
Baixe e instale em: https://www.python.org/downloads/

- `Marque a opçãp Add Path`

## 📦 Requisitos

Crie um `requirements.txt` com:

```
selenium
webdriver-manager
python-dotenv
pandas
openpyxl
```

Instale com:

```bash
pip install -r requirements.txt
```

---

## 👋 Considerações Finais

Esse projeto é um exemplo prático de como Python pode otimizar rotinas corporativas. Ele une extração automatizada de dados, tratamento e comunicação de forma segura e rastreável.

> “Automação não é apenas sobre eliminar tarefas — é sobre dar tempo ao que importa.”

---

## Autor

Fabiano Brito de Souza 
