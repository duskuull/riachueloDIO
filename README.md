# Simulando um Malware de captura de dados simples em Python e aprendendo a se proteger

Projeto desenvolvido como parte do Bootcamp Riachuel - Cibersegurança, na plataforma DIO

## Ransomware Simulado

### Descrição do Projeto

Este projeto foi desenvolvido especialmente para fins didáticos e em ambiente controlado, em um projeto do Bootcamp Riachuelo - Cibersegurança.
O objetivo foi criar uma simulação de ransomware utilizando Python, demonstrando como funcionam os processos de criptografia e descriptografia de arquivos em ataques reais.

#### Atenção!

Todo o projeto foi desenvolvido e executado em ambiente seguro e controlado, com arquivos de teste fictícios, utilizando VM (Kali).
O propósito é educacional, voltado ao aprendizado sobre criptografia, segurança da informação e defesa contra ransomware.

#### 1. Script de Criptografia - ransomware.py

O script criptografa os arquivos da pasta test_files e gera uma mensagem simulando o comportamento de um ataque ransomware.

#### Bibliotecas utilizadas

- cryptography.fernet –> fornece criptografia simétrica, permitindo criptografar e descriptografar dados com segurança.

- os –> permite navegar entre diretórios, criar e manipular arquivos no sistema operacional.

#### Descrição das funções

#### Função

**gerar_chave()**: Gera uma nova chave de criptografia e a salva no arquivo chave.key.

**carregar_chave()**: Lê e retorna a chave de criptografia salva anteriormente.

**criptografar_arquivo(arquivo, chave)**: Criptografa o conteúdo de um arquivo usando a chave gerada.

**encontrar_arquivos(diretorio)**: Percorre o diretório informado e retorna todos os arquivos a serem criptografados.

**criar_mensagem_resgate()**: Gera um arquivo LEIA ISSO.txt com uma mensagem simulada de resgate.

**main()**: Função principal que coordena todas as etapas: geração da chave, criptografia e criação da mensagem.

#### Execução:

Ao executar o script:

- Todos os arquivos dentro de test_files são criptografados.
- É criado o arquivo LEIA ISSO.txt com a mensagem de resgate simulada.
- A chave de criptografia é salva no arquivo chave.key.

#### 2. Script de Descriptografia – descriptografar.py

Este script reverte o processo de criptografia, restaurando os arquivos originais.

#### Bibliotecas utilizadas

- cryptography.fernet
- os

#### Descrição das funções

#### Função

**carregar_chave()**: Lê a chave de criptografia a partir do arquivo chave.key.
**descriptografar_arquivo(arquivo, chave)**: Descriptografa o conteúdo de um arquivo.
**encontrar_arquivos(diretorio)**: Localiza os arquivos criptografados no diretório especificado.
**main()**: Executa a rotina de descriptografia de todos os arquivos.

#### Execução

Ao rodar o script:

- Todos os arquivos criptografados dentro da pasta test_files são descriptografados.
- O conteúdo volta ao formato original e legível.

#### Conceitos Envolvidos

**- Criptografia Simétrica:**
O mesmo segredo (chave) é utilizado tanto para criptografar quanto para descriptografar os dados.

**- Ransomware:**
Tipo de malware que bloqueia o acesso a arquivos ou sistemas até que um pagamento (resgate) seja feito.

**- Chave de Criptografia:**
É o segredo matemático que garante que apenas quem a possui consiga recuperar os dados.

#### Boas Práticas e Prevenção Contra Ransomware

Embora este projeto seja apenas uma simulação, ataques reais de ransomware podem causar grandes prejuízos financeiros e operacionais.
Para se proteger, seguem algumas boas práticas recomendadas:

1. Faça backups regulares e mantenha cópias offline dos seus arquivos importantes.
2. Mantenha o sistema e softwares atualizados para corrigir vulnerabilidades conhecidas.
3. Evite abrir anexos e links suspeitos em e-mails ou mensagens.
4. Utilize antivírus e firewall sempre atualizados.
5. Desconfie de pedidos de pagamento em criptomoedas — nenhum suporte legítimo faz esse tipo de exigência.
6. Eduque colaboradores e usuários sobre engenharia social e boas práticas de cibersegurança.

## Keylogger

#### Descrição do projeto

O objetivo aqui é aprender: como funciona a captura de teclas em nível de aplicação, como registrar eventos e como pensar em defesas. O código foi executado apenas em um ambiente controlado e com contas de teste.

**Aviso legal e ético:** este projeto é estritamente para fins educacionais e de pesquisa em ambiente controlado. Não use, distribua nem execute este software em sistemas de terceiros sem autorização explícita — isso pode ser ilegal e antiético.

#### Estrutura do projeto

- keylogger.py — script que registra eventos de teclado em um arquivo de log (uso educacional).
- keylogger_email.py — (opcional) demonstração de como um log poderia ser tratado para envio automatizado em ambiente de teste. A documentação desta parte foi intencionalmente resumida por razões de segurança.

#### Requisitos

- Python
- Bibliotecas utilizadas no projeto (exemplos): pynput (monitoramento do teclado).

Observação: as bibliotecas estão listadas apenas para referência, não inclua instruções que facilitem a exfiltração de dados ou a execução do software em máquinas de terceiros.

#### Funcionamento

1. O script inicializa um listener para o teclado usando a biblioteca apropriada.
2. Quando uma tecla é pressionada, um callback registra o evento.
3. Entradas "normais" (letras, números, símbolos) são gravadas de forma legível no arquivo de log.
4. Teclas de controle (por exemplo: espaço, enter, backspace) são tratadas para representar ações correspondentes no log (pular linha, espaço, remoção, etc.).
5. Durante os testes, foi digitado no navegador e em outros aplicativos no ambiente controlado e confirmado que os eventos foram logados conforme esperado.

#### Considerações de segurança e boas práticas

- Execute experimentos apenas em máquinas dedicadas de teste/controladas.
- Mantenha registros e resultados restritos ao ambiente de laboratório.
- Analise como softwares de segurança (antivírus, EDR) detectam estes comportamentos.
- Estude medidas de mitigação: políticas de privilégio, bloqueio de software não autorizado, monitoramento de processos, restrições de execução por caminho/assinatura, e educação de usuários.
- Documente sempre autorização escrita se for necessário testar em ambientes que não são exclusivamente seus.

#### Observações finais

Este projeto foi desenvolvido exclusivamente para aprendizado em ambiente controlado.
