<div align="center">

![Upload Widget Server Banner](https://img.shields.io/badge/📤%20UPLOAD%20WIDGET%20SERVER-Cloudflare%20R2%20Integration-FF6B6B?style=for-the-badge&logo=cloudflare&logoColor=white)

**Servidor de upload de imagens com Cloudflare R2**

[📋 Sobre](#-sobre) • [🚀 Tecnologias](#-tecnologias) • [⚙️ Configuração](#️-configuração) • [📦 Instalação](#-instalação) • [🏃 Executando](#-executando) • [📚 Padrões](#-padrões) • [🔄 Fluxo](#-fluxo) • [👤 Autor](#-autor)

</div>

<div align="center">

![Static Badge](https://img.shields.io/badge/TypeScript-007ACC?style=plastic&logo=typescript&logoColor=white&labelColor=007ACC)
![Static Badge](https://img.shields.io/badge/Fastify-000000?style=plastic&logo=fastify&logoColor=white&labelColor=000000)
![Static Badge](https://img.shields.io/badge/Node.js-339933?style=plastic&logo=nodedotjs&logoColor=white&labelColor=339933)
![Static Badge](https://img.shields.io/badge/Cloudflare%20R2-F38020?style=plastic&logo=cloudflare&logoColor=white&labelColor=F38020)
![Static Badge](https://img.shields.io/badge/AWS%20SDK-232F3E?style=plastic&logo=amazonaws&logoColor=white&labelColor=232F3E)
![Static Badge](https://img.shields.io/badge/Zod-3E63DD?style=plastic&logo=zod&logoColor=white&labelColor=3E63DD)
![Static Badge](https://img.shields.io/badge/pnpm-F69220?style=plastic&logo=pnpm&logoColor=white&labelColor=F69220)

</div>

## 📋 Sobre

Servidor back-end que faz parte de uma aplicação `fullstack`</br>
desenvolvido para gerenciar uploads de imagens </br>
utilizando Cloudflare R2 como provedor de armazenamento. </br>
O projeto implementa uma API REST com Fastify, </br>
seguindo padrões de arquitetura em camadas e utilizando TypeScript para garantir type-safety.

---

> [!IMPORTANT]
> Essa aplicação trabalha em conjunto com a aplicação **front-end**</br> >
> [Front-end](https://github.com/emmanuelmarcosdeoliveira/upload-widget)

---

### Características

- ✅ Upload de imagens via stream
- ✅ Validação de arquivos (tamanho máximo: 4MB)
- ✅ Integração com Cloudflare R2
- ✅ CORS configurado
- ✅ Validação de variáveis de ambiente com Zod
- ✅ Arquitetura modular e escalável

---

## 🚀 Tecnologias

### Dependências Principais

- **[Fastify](https://www.fastify.io/)** (v5.1.0) - Framework web rápido e eficiente
- **[@aws-sdk/client-s3](https://www.npmjs.com/package/@aws-sdk/client-s3)** (v3.700.0) - Cliente S3 para integração com Cloudflare R2
- **[@aws-sdk/lib-storage](https://www.npmjs.com/package/@aws-sdk/lib-storage)** (v3.700.0) - Biblioteca para upload de streams
- **[Zod](https://zod.dev/)** (v3.23.8) - Validação de schemas TypeScript-first
- **[@fastify/multipart](https://github.com/fastify/fastify-multipart)** (v9.0.1) - Plugin para upload de arquivos
- **[@fastify/cors](https://github.com/fastify/fastify-cors)** (v10.0.1) - Plugin CORS

### Dependências de Desenvolvimento

- **[TypeScript](https://www.typescriptlang.org/)** (v5.7.2) - Superset JavaScript com tipagem estática
- **[tsx](https://github.com/esbuild-kit/tsx)** (v4.19.2) - Executor TypeScript para desenvolvimento
- **[@types/node](https://www.npmjs.com/package/@types/node)** (v22.10.0) - Tipos TypeScript para Node.js

---

## ⚙️ Configuração

### Variáveis de Ambiente

Crie um arquivo `.env` na raiz do projeto com as seguintes variáveis:

```env
CLOUDFLARE_ACCESS_KEY_ID=seu_access_key_id
CLOUDFLARE_SECRET_ACCESS_KEY=seu_secret_access_key
CLOUDFLARE_BUCKET=nome_do_seu_bucket
CLOUDFLARE_ACCOUNT_ID=seu_account_id
CLOUDFLARE_PUBLIC_URL=https://seu-dominio.com
```

### Obtendo Credenciais do Cloudflare R2

1. Acesse o [Cloudflare Dashboard](https://dash.cloudflare.com/)
2. Vá em **R2** > **Manage R2 API Tokens**
3. Crie um novo token com permissões de leitura e escrita
4. Copie o `Access Key ID` e `Secret Access Key`
5. Obtenha o `Account ID` no dashboard do Cloudflare
6. Configure o bucket e a URL pública do R2

---

## 📦 Instalação

### Pré-requisitos

- Node.js (versão 22 ou superior)
- pnpm (gerenciador de pacotes)

### Passos

1. Clone o repositório:

```bash
git clone https://github.com/emmanuelmarcosdeoliveira/upload-widget-server
cd upload-widget-server
```

2. Instale as dependências:

```bash
pnpm install
```

3. Configure as variáveis de ambiente (veja seção [Configuração](#️-configuração))

---

## 🏃 Executando

### Modo Desenvolvimento

```bash
pnpm dev
```

O servidor estará rodando em `http://localhost:3333`

### Endpoints

#### `POST /uploads`

Upload de imagem

**Request:**

- Content-Type: `multipart/form-data`
- Body: arquivo de imagem (máximo 4MB)

**Response (201):**

```json
{
  "url": "https://seu-dominio.com/nome-do-arquivo.jpg"
}
```

**Response (400):**

```json
{
  "message": "Invalid file provided." | "File size must be a maximum of 4MB.."
}
```

---

## 📚 Padrões

### Arquitetura

O projeto segue uma arquitetura em camadas:

```
src/
├── server.ts              # Configuração do servidor Fastify
├── env.ts                 # Validação de variáveis de ambiente
├── routes/                # Rotas da aplicação
│   └── upload-image.ts
├── functions/             # Casos de uso/regras de negócio
│   └── upload-image-to-storage.ts
└── storage/               # Camada de abstração de armazenamento
    ├── storage.ts         # Interface StorageProvider
    └── providers/
        └── r2-storage.ts  # Implementação para Cloudflare R2
```

### Padrões de Projeto Utilizados

- **Provider Pattern**: Abstração do provedor de armazenamento através da interface `StorageProvider`
- **Dependency Injection**: Injeção de dependências para facilitar testes e manutenção
- **Schema Validation**: Validação de variáveis de ambiente e dados com Zod
- **Stream Processing**: Upload de arquivos via streams para melhor performance

### TypeScript

- Modo `strict` habilitado
- Target: ES2022
- Module: Node16
- Lib: ES2023

---

## 🔄 Fluxo

Diagrama do fluxo principal de upload de imagens:

```mermaid
sequenceDiagram
    participant Cliente
    participant Fastify as Fastify Server
    participant Multipart as @fastify/multipart
    participant Route as uploadImageRoute
    participant UploadService as UploadImageToStorage
    participant Storage as R2StorageProvider
    participant R2 as Cloudflare R2

    Cliente->>Fastify: POST /uploads (multipart/form-data)
    Fastify->>Multipart: Processa arquivo
    Multipart->>Route: Arquivo validado (max 4MB)

    alt Arquivo inválido ou ausente
        Route->>Cliente: 400 Bad Request
    else Arquivo válido
        Route->>Storage: Instancia R2StorageProvider
        Route->>UploadService: Instancia UploadImageToStorage(storage)
        Route->>UploadService: execute({ name, contentStream, contentType })

        UploadService->>UploadService: Valida request com Zod
        UploadService->>Storage: uploadFileAsStream({ path, stream, contentType })

        Storage->>Storage: Sanitiza nome do arquivo
        Storage->>R2: Upload stream via AWS SDK
        R2-->>Storage: Upload concluído
        Storage-->>UploadService: { url }
        UploadService-->>Route: { url }
        Route->>Cliente: 201 Created { url }
    end
```

### Descrição do Fluxo

1. **Cliente** envia requisição POST com arquivo multipart
2. **Fastify** recebe e processa via plugin multipart
3. **Route** valida tamanho do arquivo (máximo 4MB)
4. **UploadImageToStorage** valida dados com Zod
5. **R2StorageProvider** sanitiza nome e faz upload para Cloudflare R2
6. **Cloudflare R2** armazena o arquivo
7. **Cliente** recebe URL pública do arquivo

---

## 👤 Autor

Desenvolvido por [Emmanuel Oliveira](https://www.linkedin.com/in/oliveira-emmanuel/) </br>
Este projeto foi desenvolvido como parte dos estudos na [RocketSeat](https://app.rocketseat.com.br).

---

## Contributors or owners

<img height="64px" src="https://res.cloudinary.com/delo0gvyb/image/upload/v1752287431/profile_mjvmdb.png"><br>

<small>Emmanuel Oliveira</small>

developed by 💫 [Emmanuel Oliveira](https://www.linkedin.com/feed/?trk=homepage-basic_sign-in-submit)<br>

&copy; Todos os Direitos Reservados

### Contribute to the projects

> Clique na seta abaixo e veja como você pode contribuir para o projeto

<details close>
<summary>Como fazer uma contribuição ao Projeto ?</summary>
 Familiarize-se com a documentação do projeto, que geralmente inclui guias de instalação.<br>
 Explore o código do projeto para entender sua estrutura e funcionamento.
 <br>

**Faça um Fork**

Crie uma cópia (fork) do repositório original em sua conta do GitHub.<br>

 <img alt="Static Badge" src="https://img.shields.io/badge/-path?style=social&logo=git&label=GitHub%20Docs&color=%23000">
 <a href="https://docs.github.com/pt/pull-requests/collaborating-with-pull-requests/working-with-forks/fork-a-repo"></a>

**Clone o Repositório**

Isso criará uma cópia local do projeto, onde você poderá fazer suas modificações.

 <img alt="Static Badge" src="https://img.shields.io/badge/-path?style=social&logo=git&label=GitHub%20Docs&color=%23000">
 <a href="https://docs.github.com/pt/repositories/creating-and-managing-repositories/cloning-a-repository"></a>

**Crie uma Nova Branch:**

Crie uma nova branch para isolar suas alterações.<br>
Isso facilita a organização do seu trabalho e a criação de pull requests.<br>

**Faça as Alterações:**

Crie funcionalidades, mude estilos ou resolva `bugs` que iram contribuir para a melhoria do Projeto.<br>

**Crie um Pull Request:**

Inclua uma descrição clara das suas alterações e explique como elas resolvem o problema ou melhoram o projeto.<br>
Solicitação: Envie um pull request para o repositório original, solicitando que suas alterações sejam incorporadas ao projeto.
<br>

**Revise e Responda a Feedback:**

Colabore: Os mantenedores do projeto podem solicitar alterações ou fornecer feedback sobre o seu código.

</details>

## Contact

[![Lindekin](https://img.shields.io/badge/--path?style=social&logo=Linkedin&logoColor=%230664C1&logoSize=auto&label=Linkedin&labelColor=%23fff&cacheSeconds=https%3A%2F%2Fwww.linkedin.com%2Fin%2Femmanuel-marcos-oliveira%2F)](https://www.linkedin.com/in/emmanuel-marcos-oliveira/)
[![WhatsApp](https://img.shields.io/badge/--path?style=social&logo=WhatsApp&logoColor=%231F3833&logoSize=auto&label=WhatsApp&color=%23fff&cacheSeconds=https%3A%2F%2Fwa.me%2F5511968336094)](https://wa.me/5511968336094)
<a href="mailto:ofs.dev.br@gmail.com"><img alt="Static Badge" src="https://img.shields.io/badge/--path?style=social&logo=Gmail&logoSize=auto&label=Gmail&cacheSeconds=--query&link=mailto%3Adev-oliveira%40outlook.com.br%22"> </a>

<sub>😁Obrigado por chegar até aqui!<sub>

## License

![Static Badge](https://img.shields.io/badge/--path?style=plastic&logo=mit&logoSize=auto&label=license%20MIT&labelColor=%23555555&color=%2397CA00)<br>
Released in 2025 This project is under the **MIT license**<br>
<br>

<div align="left">
<strong>
⭐ Se este projeto foi útil para você, considere dar uma estrela! 
</strong>

</div>
