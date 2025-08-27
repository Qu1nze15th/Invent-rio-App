# Aplicativo de Rastreamento de Inventário e Vendas

Este é um aplicativo web completo para rastreamento de inventário e vendas, desenvolvido para pequenas empresas. Ele permite o gerenciamento de ingredientes, receitas, registro de vendas e geração de relatórios.

## Visão Geral do Projeto

O aplicativo é construído com Next.js e utiliza uma arquitetura full-stack, com o frontend em React e o backend em Next.js Server Actions, interagindo com um banco de dados PostgreSQL via Prisma ORM.

## Funcionalidades Principais

- **Gerenciamento de Ingredientes**: Adicione, edite e exclua ingredientes com detalhes como nome, quantidade em estoque, unidade, custo por unidade e fornecedor. Alertas visuais para baixo estoque.
- **Gerenciamento de Receitas**: Crie receitas combinando ingredientes. O custo da receita é calculado automaticamente com base nos custos dos ingredientes.
- **Registro de Vendas**: Registre transações de vendas, que automaticamente deduzem os ingredientes utilizados do inventário.
- **Relatórios e Dashboard**: Visualize gráficos de receita semanal/mensal, as 5 receitas mais vendidas e o status do inventário (bem estocado, baixo estoque, fora de estoque).

## Pilha Tecnológica

**Frontend**:
- Next.js 14 (App Router)
- ShadCN/ui (Tailwind CSS)
- Zustand (para gerenciamento de estado, embora não totalmente implementado neste escopo inicial)
- React Hook Form + Zod (para validação de formulários, não totalmente implementado neste escopo inicial)

**Backend**:
- Next.js Server Actions (API Routes)
- PostgreSQL (Banco de Dados)
- Prisma ORM (para interação com o banco de dados)
- Next-Auth (para autenticação)

## Configuração do Projeto

Siga os passos abaixo para configurar e executar o projeto localmente.

### Pré-requisitos

Certifique-se de ter o seguinte instalado em sua máquina:

- Node.js (versão 18 ou superior)
- npm (gerenciador de pacotes do Node.js)
- Docker (para executar o banco de dados PostgreSQL)

### 1. Clonar o Repositório

```bash
git clone <URL_DO_REPOSITORIO>
cd inventory-app
```

### 2. Configurar o Banco de Dados PostgreSQL com Docker

Este projeto utiliza PostgreSQL como banco de dados. A maneira mais fácil de configurá-lo é usando Docker.

```bash
docker run --name inventory-postgres -e POSTGRES_USER=user -e POSTGRES_PASSWORD=password -e POSTGRES_DB=inventorydb -p 5432:5432 -d postgres
```

Isso iniciará um contêiner PostgreSQL com as seguintes credenciais:
- **Host**: `localhost`
- **Porta**: `5432`
- **Usuário**: `user`
- **Senha**: `password`
- **Banco de Dados**: `inventorydb`

### 3. Configurar Variáveis de Ambiente

Crie um arquivo `.env` na raiz do projeto (`inventory-app/`) e adicione a seguinte linha:

```
DATABASE_URL="postgresql://user:password@localhost:5432/inventorydb?schema=public"
NEXTAUTH_SECRET="YOUR_SECRET_HERE" # Gere uma string aleatória e segura
```

Certifique-se de substituir `YOUR_SECRET_HERE` por uma string aleatória e segura. Você pode gerar uma usando `openssl rand -base64 32` no terminal.

### 4. Instalar Dependências

Navegue até o diretório do projeto e instale as dependências:

```bash
npm install
```

### 5. Rodar Migrações do Prisma

Após configurar o banco de dados e as variáveis de ambiente, aplique as migrações do Prisma para criar as tabelas no banco de dados:

```bash
npx prisma migrate dev --name init
```

### 6. Gerar o Prisma Client

```bash
npx prisma generate
```

### 7. Executar o Aplicativo

Para iniciar o servidor de desenvolvimento:

```bash
npm run dev
```

O aplicativo estará disponível em `http://localhost:3000`.

## Testes Unitários

Para executar os testes unitários (Jest):

```bash
npm test
```

## Diagrama ER

```mermaid
erDiagram
    Ingredient ||--o{ RecipeIngredient : has
    Recipe ||--o{ RecipeIngredient : has
    Recipe ||--o{ Sale : has

    Ingredient {
        string id PK
        string name UK
        float stockQuantity
        Unit unit
        float costPerUnit
        string supplier
    }

    Recipe {
        string id PK
        string name UK
    }

    RecipeIngredient {
        string id PK
        string recipeId FK
        string ingredientId FK
        float quantity
    }

    Sale {
        string id PK
        DateTime date
        string recipeId FK
        int quantity
        float totalRevenue
    }

    Unit {
        G
        KG
        L
        ML
        UNIT
    }
```

## Postman Collection

Uma coleção do Postman para testar as APIs está disponível em `Inventory_Sales_Tracking_API.postman_collection.json` na raiz do projeto. Importe este arquivo para o Postman para acessar os endpoints de API.

## Considerações Finais

Este projeto foi desenvolvido com foco em modularidade e escalabilidade, utilizando as melhores práticas das tecnologias envolvidas. Para futuras melhorias, considere a implementação completa do Zustand e React Hook Form/Zod para gerenciamento de estado e validação de formulários no frontend, bem como aprimoramentos na interface do usuário e na experiência do usuário.


