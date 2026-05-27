# PostgreSQL Vetorizado com pgvector
## Autor: Diego Augusto

Este projeto tem como objetivo disponibilizar uma infraestrutura Plug & Play utilizando PostgreSQL com suporte a embeddings de Inteligência Artificial através da extensão pgvector.

A aplicação foi desenvolvida utilizando Docker Compose, permitindo que qualquer sistema baseado em IA, RAG (Retrieval-Augmented Generation) ou busca semântica consiga subir rapidamente um banco de dados vetorial totalmente funcional.

---

# Tecnologias Utilizadas

- PostgreSQL 16
- pgvector
- Docker
- Docker Compose

---

# Objetivo do Projeto

Sistemas modernos de Inteligência Artificial utilizam embeddings vetoriais para realizar buscas semânticas e recuperação de contexto.

O pgvector adiciona suporte a operações matemáticas vetoriais dentro do PostgreSQL, permitindo armazenar e consultar embeddings diretamente no banco de dados.

Este projeto entrega uma infraestrutura pronta para uso, facilitando integração com aplicações de IA generativa.

---

# Estrutura do Projeto

```bash
ia-vector-db/
├── docker-compose.yml
├── init.sql
└── README.md
```

---

# Como subir o projeto

Abra o terminal na pasta do projeto e execute:

```bash
docker compose up -d
```

Esse comando irá:

- baixar a imagem pgvector/pgvector:pg16
- criar o container PostgreSQL
- executar automaticamente o init.sql
- iniciar o banco vetorial
- criar persistência de dados

---

# Como verificar se o container está rodando

```bash
docker ps
```

O container deverá aparecer com o nome:

```bash
ia-vector-db
```

---

# Como verificar se o pgvector está ativo

```bash
docker exec -it ia-vector-db psql -U admin -d vector_db -c "\dx"
```

A extensão `vector` deve aparecer na lista.

---

# Como parar o projeto

```bash
docker compose down
```

Esse comando:

- para o container
- remove o container
- mantém os dados persistidos

---

# Como remover completamente o projeto

```bash
docker compose down -v
```

Esse comando remove:

- container
- volumes
- banco de dados
- persistência dos dados

---



# Arquivo docker-compose.yml

Responsável por:

- subir o container PostgreSQL
- configurar persistência de dados
- mapear portas
- executar automaticamente o init.sql
- utilizar a imagem oficial pgvector/pgvector:pg16

---

# Arquivo init.sql

Executado automaticamente na primeira inicialização do container.

Responsável por:

- ativar a extensão pgvector
- criar tabela de exemplo
- criar coluna do tipo VECTOR(1536)

---

# Exemplo de Estrutura SQL

```sql
CREATE EXTENSION IF NOT EXISTS vector;

CREATE TABLE embeddings_ia (
    id SERIAL PRIMARY KEY,
    pergunta TEXT,
    embedding VECTOR(1536)
);
```

---

# Persistência de Dados

O volume Docker garante que os dados permaneçam salvos mesmo após reinicialização do container.

Volume utilizado:

```yaml
postgres_data:/var/lib/postgresql/data
```

---

# Troubleshooting

## Porta 5432 já está em uso

Caso exista outro PostgreSQL rodando localmente:

Altere no docker-compose.yml:

```yaml
ports:
  - "5433:5432"
```

Depois execute novamente:

```bash
docker compose down -v
docker compose up -d
```

---

## Extensão vector não encontrada

Verifique se está utilizando:

```yaml
pgvector/pgvector:pg16
```

Não utilize:

```yaml
postgres:latest
```

---

## init.sql não executou

Isso acontece quando o container já havia sido criado anteriormente.

Execute:

```bash
docker compose down -v
docker compose up -d
```

---

## Banco recusando conexão

O PostgreSQL pode levar alguns segundos para inicializar completamente.

Aguarde aproximadamente 5 segundos e tente novamente.

---

# Conceitos Aplicados

Este projeto aplica conceitos estudados em Arquitetura em Nuvem:

- Cloud Native
- Infraestrutura Plug & Play
- Containers
- Persistência de Dados
- Banco Vetorial
- Embeddings
- RAG (Retrieval-Augmented Generation)
- Docker Compose
- Infraestrutura como Código

---

# Resultado Esperado

Após executar:

```bash
docker compose up -d
```

O sistema deve:

- criar automaticamente o banco PostgreSQL
- ativar o pgvector
- criar tabela vetorial
- permitir armazenamento de embeddings
- manter persistência dos dados

---

# Repositório Desenvolvido por

Diego Augusto