# Clyvo VitalPet API

API REST desenvolvida em **Java 17 + Spring Boot** para atender aos requisitos da entrega de **Java Advanced - 1ª Sprint**.

A solução simula uma plataforma para clínicas veterinárias acompanharem pets após consultas, gerando alertas de retorno e oferecendo um dashboard com indicadores principais. Assim, o projeto vai além de um CRUD simples e aplica regras de negócio dentro do contexto proposto.

## Requisitos atendidos

- Aplicação Java com Spring Boot.
- Persistência em SGBD relacional com H2 em memória.
- Entidades relacionadas e mapeadas com JPA.
- Programação Orientada a Objetos com separação por camadas.
- Controllers RESTful.
- Services com regras de negócio.
- Repositories com Spring Data JPA.
- DTOs para entrada e saída de dados.
- Validação de campos com Bean Validation.
- Paginação de resultados.
- Ordenação de resultados.
- Busca com parâmetros.
- Cache para otimização de consultas.
- Tratamento global de erros e exceções.
- Documentação com Swagger/OpenAPI.
- Collection do Postman para testar os endpoints.
- Uso de padrões de projeto como Controller-Service-Repository, DTO e Mapper.

## Tecnologias

- Java 17
- Spring Boot 3.3.5
- Spring Web
- Spring Data JPA
- Bean Validation
- Spring Cache
- H2 Database
- Springdoc OpenAPI / Swagger
- Maven

## Como executar

Abra o projeto no IntelliJ e rode a classe:

```text
src/main/java/com/clyvo/vitalpet/ClyvoVitalpetApplication.java
```

Ou execute pelo terminal:

```bash
mvn spring-boot:run
```

A API iniciará em:

```text
http://localhost:8080
```

## Swagger

Após iniciar o projeto, acesse:

```text
http://localhost:8080/swagger-ui.html
```

## H2 Console

Acesse:

```text
http://localhost:8080/h2-console
```

Configuração:

```text
JDBC URL: jdbc:h2:mem:vitalpetdb
User: sa
Password: deixe vazio
```

## Principais endpoints

### Clínicas

```text
POST   /api/clinicas
GET    /api/clinicas/{id}
GET    /api/clinicas?nome=vet&page=0&size=10&sortBy=nome&direction=asc
PUT    /api/clinicas/{id}
DELETE /api/clinicas/{id}
PATCH  /api/clinicas/{id}/ativar
```

### Tutores

```text
POST   /api/tutores
GET    /api/tutores/{id}
GET    /api/tutores?nome=eduardo&cpf=12345678901&page=0&size=10
PUT    /api/tutores/{id}
DELETE /api/tutores/{id}
PATCH  /api/tutores/{id}/ativar
```

### Pets

```text
POST   /api/pets
GET    /api/pets/{id}
GET    /api/pets?nome=thor&especie=cachorro&tutorId=1&page=0&size=10
PUT    /api/pets/{id}
DELETE /api/pets/{id}
PATCH  /api/pets/{id}/ativar
```

### Veterinários

```text
POST   /api/veterinarios
GET    /api/veterinarios/{id}
GET    /api/veterinarios?especialidade=clinico&clinicaId=1&page=0&size=10
PUT    /api/veterinarios/{id}
DELETE /api/veterinarios/{id}
PATCH  /api/veterinarios/{id}/ativar
```

### Consultas

```text
POST  /api/consultas
GET   /api/consultas/{id}
GET   /api/consultas?petId=1&status=AGENDADA&page=0&size=10&sortBy=dataHora&direction=desc
PATCH /api/consultas/{id}
PATCH /api/consultas/{id}/finalizar
PATCH /api/consultas/{id}/cancelar
```

Ao finalizar uma consulta com `descricaoAcompanhamento`, o sistema cria automaticamente um acompanhamento e um alerta de retorno para 7 dias depois.

### Acompanhamentos

```text
POST  /api/acompanhamentos
GET   /api/acompanhamentos/{id}
GET   /api/acompanhamentos?petId=1&status=ATIVO&page=0&size=10
PATCH /api/acompanhamentos/{id}/concluir
PATCH /api/acompanhamentos/{id}/cancelar
```

### Alertas

```text
POST  /api/alertas
GET   /api/alertas/{id}
GET   /api/alertas?petId=1&status=PENDENTE&prioridade=ALTA&page=0&size=10
PATCH /api/alertas/{id}/resolver
PATCH /api/alertas/{id}/cancelar
```

### Dashboard

```text
GET /api/dashboard/resumo
```

## Exemplo de payloads

### Criar clínica

```json
{
  "nome": "VitalPet Vila Mariana",
  "endereco": "Rua das Flores, 100",
  "cidade": "São Paulo",
  "estado": "SP",
  "cep": "04000000",
  "telefone": "11999990000",
  "email": "contato@vitalpet.com",
  "cnpj": "12345678000199"
}
```

### Criar tutor

```json
{
  "nome": "Eduardo Martins",
  "email": "eduardo@email.com",
  "telefone": "11988887777",
  "cpf": "12345678901",
  "endereco": "Rua Exemplo, 50",
  "cidade": "São Paulo",
  "estado": "SP",
  "cep": "01001000"
}
```

### Criar pet

```json
{
  "nome": "Thor",
  "especie": "Cachorro",
  "raca": "Golden Retriever",
  "dataNascimento": "2021-05-10",
  "sexo": "Macho",
  "peso": 28.5,
  "observacoes": "Pet dócil e vacinado",
  "tutorId": 1
}
```

### Criar veterinário

```json
{
  "nome": "Dra. Ana Souza",
  "email": "ana@vitalpet.com",
  "telefone": "11977776666",
  "crmv": "SP-12345",
  "especialidade": "Clínica Geral",
  "clinicaId": 1
}
```

### Criar consulta

```json
{
  "dataHora": "2030-06-10T14:30:00",
  "tipo": "Rotina",
  "sintomas": "Check-up anual",
  "diagnostico": "Aguardando avaliação",
  "tratamento": "A definir",
  "valor": 180.00,
  "petId": 1,
  "veterinarioId": 1
}
```

### Finalizar consulta e gerar acompanhamento automático

```json
{
  "diagnostico": "Pet saudável, apenas com leve irritação na pele",
  "tratamento": "Pomada dermatológica por 7 dias",
  "descricaoAcompanhamento": "Monitorar evolução da irritação e confirmar melhora com o tutor"
}
```

## Testes pelo Postman

A collection está na pasta:

```text
postman/Clyvo-VitalPet.postman_collection.json
```

Importe essa collection no Postman ou Insomnia, inicie a aplicação e execute as requisições na ordem sugerida.
