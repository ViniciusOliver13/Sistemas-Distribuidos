# Aula 1: Deploy de Aplicação Web na Nuvem

Passo a passo do processo completo para implantar uma aplicação web (React + Vite) em um servidor na nuvem (AWS EC2 com Linux Ubuntu e Apache2).

## Fase 1: Configurando o Servidor na Nuvem (AWS)

O primeiro passo é preparar a infraestrutura onde nosso site ficará hospedado.

### 1.1. Criação da Máquina Virtual (EC2)

  - **Provedor:** AWS (Amazon Web Services)
  - **Serviço:** EC2 (Elastic Compute Cloud)
  - **Sistema Operacional:** Linux Ubuntu
  - **Acesso:** Realizado de forma remota via SSH, usando uma chave de segurança (`.pem`).

### 1.2. Instalando o Servidor Web (Apache2)

Após conectar ao servidor, instalamos o Apache, que é o software que irá receber os visitantes e entregar os arquivos do nosso site.

```bash
# Atualiza a lista de pacotes do sistema
sudo apt update

# Instala o Apache2
sudo apt install apache2 -y
```

## Fase 2: Desenvolvendo a Aplicação Localmente

Com o servidor pronto, o próximo passo é criar a aplicação na nossa máquina de desenvolvimento.

### 2.1. Criação do Projeto com Vite

Usamos o Vite para criar um projeto React de forma rápida e moderna.

```bash
# Cria um novo projeto
npm create vite@latest
```

### 2.2. Instalação das Dependências

O comando `npm install` é **crucial**. Ele baixa todas as ferramentas (`vite`, `react`, etc.) que o projeto precisa para funcionar.

```bash
npm install
```

> **Nota:** Se comandos como `npm run build` falharem com o erro `vite: not found`, é quase certeza que o `npm install` não foi executado.

### 2.3. O Servidor de Desenvolvimento

Para visualizar o site **enquanto programamos**, usamos o servidor de desenvolvimento do Vite.

```bash
npm run dev
```

  - Ele inicia um servidor local (ex: `http://localhost:5173`).
  - Atualiza a página automaticamente a cada alteração no código.
  - Para parar o servidor, use o atalho `Ctrl + C` no terminal.

## Fase 3: O Processo de "Build" e Teste

Depois de desenvolver, precisamos "empacotar" nosso projeto em arquivos estáticos que o Apache possa entender.

### 3.1. Gerando a Build

O comando `build` transforma nosso código React em HTML, CSS e JavaScript otimizados.

```bash
npm run build
```

  - Este comando cria uma nova pasta chamada `dist/`.
  - É **todo o conteúdo da pasta `dist/`** que será enviado ao servidor.

### 3.2. Testando a Build Localmente

Não podemos simplesmente abrir o `dist/index.html` no navegador (resulta em página branca). O jeito correto de testar a build é com o comando `preview` do Vite.

```bash
npm run preview
```

  - Ele inicia um servidor local (ex: `http://localhost:4174`) que simula o ambiente de produção.
  - Se o site funcionar aqui, ele funcionará no Apache.

## Fase 4: Deploy no Servidor Apache

Com a pasta `dist/` pronta e testada, é hora da publicação final.

### 4.1. Transferindo os Arquivos (scp)

Usamos o `scp` (Secure Copy) no **nosso terminal local** para enviar a pasta `dist` para o servidor.

```bash
# Estrutura do comando:
# scp -i [chave] -r [pasta_local] [usuario@ip_servidor]:[pasta_remota]

scp -i ~/caminho/para/sua-chave.pem -r dist/ ubuntu@SEU_IP_DA_AWS:~/
```

  - `-i`: Especifica o arquivo da sua chave de acesso.
  - `-r`: Recursivo, para copiar a pasta inteira.
  - `~/`: É um atalho para a pasta principal do usuário no servidor.

### 4.2. Publicando no Apache (mv)

Após o envio, conectamos ao **servidor AWS via SSH** e movemos os arquivos para o lugar certo.

```bash
# 1. (Opcional, mas recomendado) Limpa o conteúdo padrão do Apache.
sudo rm -rf /var/www/html/*

# 2. Move o CONTEÚDO da pasta dist para a raiz da web.
sudo mv ~/dist/* /var/www/html/
```

> **Atenção:** O `/*` no final de `~/dist/*` é muito importante. Ele garante que você mova os arquivos *de dentro* da pasta `dist`, e não a pasta `dist` em si.

## Cheatsheet: Comandos e Diretórios Essenciais

### Comandos na Máquina Local

  - `npm run dev`: Inicia o servidor de desenvolvimento.
  - `npm run build`: Gera a pasta `dist` para produção.
  - `npm run preview`: Testa a pasta `dist` localmente.
  - `scp ...`: Envia arquivos para o servidor.

### Comandos no Servidor (Linux)

  - `sudo [comando]`: Executa um comando como administrador.
  - `mv [origem] [destino]`: Move ou renomeia arquivos/pastas.
  - `cp [origem] [destino]`: Copia arquivos/pastas.
  - `rm [arquivo]`: Remove um arquivo.
  - `rm -r [pasta]`: Remove uma pasta e seu conteúdo (Use com **extremo cuidado**).
  - `ls -la`: Lista arquivos e pastas com detalhes e permissões.

### Atalhos de Terminal

  - `Ctrl + C`: Interrompe um processo em execução (como o `npm run dev`).

### Diretórios Importantes no Servidor

  - `/var/www/html/`: A "raiz da web" do Apache. Os arquivos do seu site ficam aqui.
  - `/etc/apache2/`: Onde ficam os arquivos de configuração do Apache.