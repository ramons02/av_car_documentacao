# API AV-CAR

Documentação da API REST do sistema **AV-CAR** (backend em `av_car_api`, executado pela `av_car_app`). Descrição dos endpoints conforme os controllers (`br.edu.senai.fatesg.avcar.business.*`).

## Acesso

| Item        | Valor                                              |
|-------------|----------------------------------------------------|
| Base URL    | `http://localhost:8080`                            |
| Prefixo API | `/api`                                             |
| Formato     | JSON (`application/json`)                          |
| Documentação interativa | `http://localhost:8080/swagger-ui/index.html` |

Para acessar, a aplicação precisa estar rodando (ver o guia de execução no README da `av_car_app`). A porta e o banco são definidos no `av_car_infra/.env`.

## Padrão de resposta

Os endpoints retornam o recurso diretamente em JSON, com status HTTP padrão:

| Status | Significado                          |
|--------|--------------------------------------|
| 200    | Sucesso (busca, atualização, ações)  |
| 201    | Recurso criado                       |
| 204    | Excluído (sem corpo)                 |
| 400    | Requisição inválida                  |
| 404    | Recurso não encontrado               |
| 500    | Erro interno                         |

Existe também o envelope `ApiResponse<T>` com os campos `sucesso`, `mensagem`, `dados` e `erros`, usado para padronizar comunicação de erros.

## Clientes

Base: `/api/clientes`

| Método | Rota                         | Descrição                            |
|--------|------------------------------|--------------------------------------|
| POST   | `/api/clientes/pf`           | Cria cliente pessoa física            |
| POST   | `/api/clientes/pj`           | Cria cliente pessoa jurídica          |
| PUT    | `/api/clientes/{id}`         | Atualiza dados do cliente             |
| GET    | `/api/clientes/buscar?nome=` | Busca clientes por nome               |

### POST `/api/clientes/pf`

```json
{
  "nome": "João da Silva",
  "endereco": "Rua A, 100",
  "bairro": "Centro",
  "cidade": "Goiânia",
  "estado": "GO",
  "cep": "74000-000",
  "telefone": "(62) 99999-0000",
  "email": "joao@email.com",
  "cpf": "123.456.789-00",
  "rg": "12.345.678",
  "dataNascimento": "1990-01-15",
  "observacoes": ""
}
```

### POST `/api/clientes/pj`

```json
{
  "nome": "Auto Peças Central",
  "endereco": "Av. B, 200",
  "bairro": "Jardim",
  "cidade": "Goiânia",
  "estado": "GO",
  "cep": "74000-001",
  "telefone": "(62) 3222-0000",
  "email": "contato@autopecas.com",
  "cnpj": "12.345.678/0001-90",
  "inscricaoEstadual": "10.123.456-7",
  "razaoSocial": "Auto Peças Central Ltda",
  "observacoes": ""
}
```

### PUT `/api/clientes/{id}`

```json
{
  "nome": "João da Silva",
  "telefone": "(62) 98888-0000",
  "email": "joao2@email.com"
}
```

## Colaboradores

Base: `/api/colaboradores`

| Método | Rota                              | Descrição                     |
|--------|-----------------------------------|-------------------------------|
| GET    | `/api/colaboradores/buscar?nome=` | Busca colaborador por nome     |
| POST   | `/api/colaboradores`              | Cria colaborador               |
| PUT    | `/api/colaboradores/{id}`         | Atualiza colaborador           |
| GET    | `/api/colaboradores/funcoes`      | Lista funções/cargos existentes |

### POST `/api/colaboradores`

```json
{
  "nome": "Maria Souza",
  "cpf": "987.654.321-00",
  "ddi1": "55",
  "ddd1": "62",
  "numerotelefone1": "99999-1111",
  "email": "maria@email.com",
  "funcaoIds": [1, 2]
}
```

## Fornecedores

Base: `/api/fornecedores`

| Método | Rota                            | Descrição                    |
|--------|---------------------------------|------------------------------|
| GET    | `/api/fornecedores/buscar?nome=`| Busca fornecedor por nome    |
| POST   | `/api/fornecedores`             | Cria fornecedor              |
| PUT    | `/api/fornecedores/{id}`        | Atualiza fornecedor          |

### POST `/api/fornecedores`

```json
{
  "razaoSocial": "Distribuidora de Peças Ltda",
  "cnpj": "98.765.432/0001-10",
  "ddi": "55",
  "ddd": "62",
  "numeroFornecedor": "3222-1111",
  "email": "vendas@distribuidora.com",
  "enderecoFornecedor": "Rua C, 300",
  "bairroFornecedor": "Setor Sul",
  "cidadeFornecedor": "Goiânia",
  "estadoFornecedor": "GO",
  "cepFornecedor": 74000100
}
```

## Parceiros (Externos)

Base: `/api/parceiros`

| Método | Rota                        | Descrição                 |
|--------|-----------------------------|---------------------------|
| GET    | `/api/parceiros/buscar?nome=`| Busca parceiro por nome   |
| POST   | `/api/parceiros`            | Cria parceiro             |
| PUT    | `/api/parceiros/{id}`       | Atualiza parceiro         |

### POST `/api/parceiros`

```json
{
  "nome": "Oficina Bom Amigo",
  "cnpj": "11.222.333/0001-44",
  "tipoServico": "Funilaria e Pintura",
  "telefone": "(62) 3555-2222",
  "email": "contato@bomamigo.com",
  "ativo": true
}
```

## Peças

Base: `/api/pecas`

| Método | Rota                                        | Descrição                          |
|--------|---------------------------------------------|-----------------------------------|
| GET    | `/api/pecas/buscar?codigo=`                 | Busca peça por código              |
| GET    | `/api/pecas/estoque-baixo?min=5`            | Lista peças com estoque baixo      |
| POST   | `/api/pecas`                                | Cria peça                          |
| PUT    | `/api/pecas/{id}`                           | Atualiza peça                      |
| DELETE | `/api/pecas/{id}`                           | Remove peça                        |

### POST `/api/pecas`

```json
{
  "codigoNacional": 11885512,
  "codigoInterno": "FC-001",
  "nome": "Pastilha de freio dianteira",
  "descricao": "Jogo com 4 pastilhas",
  "fabricante": "Fras-Le",
  "categoria": "Freios",
  "precoCusto": 45.00,
  "precoVenda": 89.90,
  "quantidadeEstoque": 20,
  "garantiaPeca": 90,
  "dataCompraPeca": "2026-01-10",
  "fornecedorId": 1
}
```

## Serviços

Base: `/api/servicos`

| Método | Rota                         | Descrição                  |
|--------|------------------------------|----------------------------|
| GET    | `/api/servicos/buscar?nome=` | Busca serviço por nome      |
| POST   | `/api/servicos`              | Cria serviço                |
| PUT    | `/api/servicos/{id}`         | Atualiza serviço            |

### POST `/api/servicos`

```json
{
  "nomeServico": "Troca de óleo e filtros",
  "descricaoServico": "Troca de óleo do motor e filtro de óleo",
  "valorServico": 120.00,
  "garantiaDias": 30,
  "tempoEstimado": "01:00"
}
```

## Veículos

Base: `/api/veiculos`

| Método | Rota                                          | Descrição                          |
|--------|-----------------------------------------------|------------------------------------|
| GET    | `/api/veiculos/buscar?placa=`                 | Busca veículo por placa             |
| GET    | `/api/veiculos/cliente/{clienteId}`           | Lista veículos de um cliente        |
| POST   | `/api/veiculos`                               | Cria veículo                        |
| PUT    | `/api/veiculos/{id}`                          | Atualiza veículo                    |
| GET    | `/api/veiculos/marcas`                        | Lista marcas disponíveis            |
| GET    | `/api/veiculos/marcas/{marcaId}/modelos`      | Lista modelos de uma marca          |

### POST `/api/veiculos`

```json
{
  "placa": "ABC-1D23",
  "chassi": "9BWZZZ377VT004251",
  "anoFabricacao": 2020,
  "anoModelo": 2021,
  "cor": "Prata",
  "quilometragem": 45000,
  "acessorios": "Ar condicionado, alarme",
  "modeloId": 1,
  "clienteId": 1
}
```

## Ordens de Serviço

Base: `/api/ordens-servico`

### Consultas

| Método | Rota                          | Descrição                       |
|--------|-------------------------------|--------------------------------|
| GET    | `/api/ordens-servico`         | Lista todas as OS              |
| GET    | `/api/ordens-servico/{id}`    | Busca uma OS por id            |
| GET    | `/api/ordens-servico/status/{status}` | Busca OS por status   |
| GET    | `/api/ordens-servico/dashboard/resumo` | KPIs do painel inicial |

Status possíveis: `Aberta`, `Em orçamento`, `Aguardando peça`, `Em execução`, `Finalizada`, `Cancelada`.

### Criação e atualização

| Método | Rota                   | Descrição                 |
|--------|------------------------|--------------------------|
| POST   | `/api/ordens-servico`  | Cria uma OS              |
| PATCH  | `/api/ordens-servico/{id}` | Atualiza defeito e forma de pagamento |
| DELETE | `/api/ordens-servico/{id}` | Exclui uma OS           |

#### POST `/api/ordens-servico`

```json
{
  "veiculoId": 1,
  "responsavelId": 2,
  "entradaVeiculo": "2026-01-20",
  "defeitoRelatado": "Barulho no motor ao acelerar",
  "formaPagamento": "A VISTA"
}
```

#### PATCH `/api/ordens-servico/{id}`

```json
{
  "defeitoRelatado": "Barulho no motor corrigido, agora falha ao dar partida",
  "formaPagamento": "CARTÃO"
}
```

### Fluxo de status

| Método | Rota                                | Descrição                          |
|--------|-------------------------------------|-----------------------------------|
| POST   | `/api/ordens-servico/{id}/avancar/orcamento`   | Marca OS como em orçamento |
| POST   | `/api/ordens-servico/{id}/avancar/execucao`    | Marca OS como em execução |
| POST   | `/api/ordens-servico/{id}/avancar/pagamento`   | Avança para etapa de pagamento |
| POST   | `/api/ordens-servico/{id}/avancar/finalizar`   | Finaliza a OS             |
| POST   | `/api/ordens-servico/{id}/cancelar`            | Cancela a OS              |
| POST   | `/api/ordens-servico/{id}/pausar`              | Pausa a OS                |
| POST   | `/api/ordens-servico/{id}/retornar`            | Retorna a OS para status anterior |

### Itens, peças, descontos e garantia

| Método | Rota                                        | Descrição                          |
|--------|---------------------------------------------|-----------------------------------|
| GET    | `/api/ordens-servico/{id}/itens-servico`    | Lista itens de serviço da OS      |
| POST   | `/api/ordens-servico/{id}/itens-servico`    | Adiciona item de serviço          |
| DELETE | `/api/ordens-servico/{id}/itens-servico/{itemId}` | Remove item de serviço     |
| GET    | `/api/ordens-servico/{id}/itens-peca`       | Lista itens de peça da OS         |
| POST   | `/api/ordens-servico/{id}/itens-peca`       | Adiciona item de peça             |
| DELETE | `/api/ordens-servico/{id}/itens-peca/{itemId}`   | Remove item de peça         |
| GET    | `/api/ordens-servico/{id}/servicos-externos`| Lista serviços externos da OS     |
| POST   | `/api/ordens-servico/{id}/servicos-externos`| Adiciona serviço externo           |
| DELETE | `/api/ordens-servico/{id}/servicos-externos/{itemId}` | Remove serviço externo |
| POST   | `/api/ordens-servico/{id}/garantia?dias=90` | Aplica garantia estendida          |
| GET    | `/api/ordens-servico/{id}/garantia`         | Calcula garantia da OS             |
| POST   | `/api/ordens-servico/{id}/desconto?valor=50`| Aplica desconto na OS              |

#### POST `/api/ordens-servico/{id}/itens-servico`

```json
{
  "servicoId": 1,
  "quantidade": 1,
  "valorUnitario": 120.00,
  "horaInicio": "09:00",
  "horaFim": "10:00",
  "status": "Concluído",
  "colaboradorId": 3
}
```

#### POST `/api/ordens-servico/{id}/itens-peca`

```json
{
  "pecaId": 1,
  "quantidade": 2,
  "valorUnitario": 89.90
}
```

#### POST `/api/ordens-servico/{id}/servicos-externos`

```json
{
  "fornecedorId": 2,
  "descricao": "Retífica do motor",
  "valor": 850.00,
  "garantiaDias": 30
}
```

## Modelo principal de resposta (Ordem de Serviço)

Os endpoints de OS retornam o DTO com estes campos:

| Campo               | Tipo          | Descrição                          |
|---------------------|---------------|-----------------------------------|
| id                  | long          | Identificador da OS               |
| numeroOs            | integer       | Número legado da OS               |
| veiculo             | string        | Placa e modelo (ex.: "ABC-1D23 - Corsa") |
| status              | string        | Status atual da OS                |
| dataAbertura        | date-time     | Data/hora de abertura             |
| dataFinalizacao     | date-time     | Data/hora de finalização          |
| entradaVeiculo      | date          | Data de entrada do veículo        |
| defeitoRelatado     | string        | Defeito informado                 |
| quantidadePecas     | integer       | Quantidade de peças usadas        |
| valorTotalPecas     | number        | Custo total de peças              |
| valorMaoObra        | number        | Mão de obra                       |
| valorServicoExterno | number        | Serviços externos                 |
| formaPagamento      | string        | Forma de pagamento                |
| valorDesconto       | number        | Desconto aplicado                 |
| valorTotal          | number        | Valor total da OS                 |
| garantia            | integer       | Dias de garantia                  |
| colaboradorNome     | string        | Nome do responsável               |
| ativo               | boolean       | Se a OS está ativa                |

## Como testar

1. Suba a aplicação seguindo o guia do README da `av_car_app`.
2. Acesse o Swagger UI em `http://localhost:8080/swagger-ui/index.html` ou use uma ferramenta como Postman/Insomnia.
3. Exemplo rápido:

```
GET http://localhost:8080/api/ordens-servico/dashboard/resumo
```

Resposta:

```json
{
  "totalOS": 8,
  "osAbertas": 3,
  "faturamentoTotal": 12450.50,
  "descontosTotal": 200.00
}
```

> O Swagger UI é gerado pela dependência `springdoc-openapi-starter-webmvc-ui` (declarada no pom da `av_car_app`), que expõe também o OpenAPI JSON em `http://localhost:8080/v3/api-docs`.