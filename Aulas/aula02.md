# Guia da 2ª Aula de Sistemas Distribuídos

Este arquivo serve como um resumo e guia prático dos conceitos e ferramentas abordados na segunda aula da disciplina de Sistemas Distribuídos.

## 🚀 Aprendizados e Conceitos da Aula

Os principais pontos teóricos discutidos foram:

* **Limitações de Sistemas Centralizados:** Embora sejam mais simples de projetar e manter inicialmente, os sistemas centralizados enfrentam desafios significativos com o crescimento (escalabilidade). Aumentar a carga pode levar a gargalos, e a existência de um ponto único de falha os torna menos resilientes.
* **Boas Práticas de Instalação:** Foi reforçada a importância de seguir as instruções do repositório da disciplina e, principalmente, a documentação oficial das ferramentas. Isso garante que o ambiente de desenvolvimento seja consistente e evita problemas de compatibilidade ou configuração.

---

## ⚙️ Guia Prático: Fazendo Deploy com AWS Amplify

Nesta aula, aprendemos uma forma rápida de fazer deploy de uma aplicação web estática (como um projeto feito em Vue, React, Angular, etc.) utilizando o serviço **AWS Amplify**.

### ⚠️ Pré-requisitos: Preparando o Projeto

Antes de fazer o deploy na AWS, seu projeto precisa ser preparado. Siga os dois passos abaixo:

1.  **Build do Projeto:**
    É necessário gerar a versão de produção da sua aplicação. Este processo compila e otimiza seu código, geralmente colocando o resultado em uma pasta chamada `dist`.

    ```bash
    # Usando NPM para gerenciar pacotes
    npm run build
    ```
2.  **Compactar os Arquivos:**
    Após o comando `build` ser concluído, uma pasta `dist` será criada na raiz do seu projeto.
    **Observação importante:** Entre na pasta `dist`, selecione **TODOS** os arquivos e pastas dentro dela e compacte-os em um único arquivo `.zip`. Não compacte a pasta `dist` por fora, mas sim o seu conteúdo.

### 📋 Passo a Passo para o Deploy via Upload

Com o arquivo `.zip` pronto, siga os seguintes passos no console da AWS:

1.  **Acessar o AWS Amplify:**
    No console da AWS, procure e acesse o serviço **AWS Amplify**. Clique em **"Hospedar um aplicativo da Web"** (ou uma opção similar como "Get Started" ou "Deploy on app").

2.  **Selecionar o Método de Deploy:**
    O Amplify oferecerá diferentes formas de fazer o deploy. As mais comuns são conectar a um repositório Git (GitHub, GitLab, etc.) ou fazer o upload manual. Para o método visto em aula, escolha a opção **"Implantar sem um provedor Git"** (Deploy without a Git provider).

3.  **Fazer o Upload do Arquivo:**
    Na tela seguinte, você encontrará uma área para arrastar e soltar arquivos. Mova o seu arquivo `.zip` (que você criou a partir da pasta `dist`) para esta área indicada.

4.  **Concluir:**
    Após o upload, basta seguir as instruções finais (geralmente nomeando o aplicativo) para que a AWS comece o processo de deploy. Em poucos minutos, seu site estará no ar com um link fornecido pelo Amplify!
