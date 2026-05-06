# API Documentation - GestaoELMO

## 📋 Visão Geral

A API RESTful do GestaoELMO fornece endpoints para gerenciar produtos, movimentações de estoque, alertas e notificações.

**Base URL**: `http://localhost:5000/api`

## 🔐 Autenticação

Todos os endpoints requerem um token JWT no header:
```
Authorization: Bearer {token}
```

---

## 📦 Produtos

### Listar Produtos
```http
GET /products?page=1&limit=10&category=Periféricos
```

**Response (200)**:
```json
{
  "data": [
    {
      "id": "507f1f77bcf86cd799439011",
      "name": "Mouse Óptico",
      "sku": "MOUSE-001",
      "category": "Periféricos",
      "quantity": 45,
      "minStock": 10,
      "maxStock": 200,
      "price": 49.90,
      "createdAt": "2026-05-06T15:00:00Z"
    }
  ],
  "pagination": {
    "page": 1,
    "limit": 10,
    "total": 1
  }
}
```

### Criar Produto
```http
POST /products
Content-Type: application/json

{
  "name": "Teclado Mecânico",
  "sku": "KEYB-001",
  "category": "Periféricos",
  "minStock": 5,
  "maxStock": 100,
  "price": 299.90,
  "unit": "un"
}
```

**Response (201)**:
```json
{
  "id": "507f1f77bcf86cd799439012",
  "name": "Teclado Mecânico",
  "sku": "KEYB-001",
  "category": "Periféricos",
  "quantity": 0,
  "minStock": 5,
  "maxStock": 100,
  "price": 299.90,
  "unit": "un",
  "createdAt": "2026-05-06T15:00:00Z"
}
```

### Obter Produto
```http
GET /products/:id
```

### Atualizar Produto
```http
PUT /products/:id
Content-Type: application/json

{
  "name": "Teclado Mecânico RGB",
  "price": 349.90
}
```

### Deletar Produto
```http
DELETE /products/:id
```

---

## 📊 Estoque

### Adicionar Estoque
```http
POST /stock/add
Content-Type: application/json

{
  "productId": "507f1f77bcf86cd799439011",
  "quantity": 50,
  "reason": "purchase",
  "notes": "Pedido #12345"
}
```

**Response (200)**:
```json
{
  "success": true,
  "product": {
    "id": "507f1f77bcf86cd799439011",
    "quantity": 95
  },
  "movement": {
    "id": "507f1f77bcf86cd799439013",
    "type": "IN",
    "quantity": 50,
    "previousQuantity": 45,
    "newQuantity": 95,
    "reason": "purchase",
    "createdAt": "2026-05-06T15:05:00Z"
  }
}
```

### Remover Estoque
```http
POST /stock/remove
Content-Type: application/json

{
  "productId": "507f1f77bcf86cd799439011",
  "quantity": 5,
  "reason": "sale",
  "notes": "Venda PDV"
}
```

### Ajustar Estoque
```http
POST /stock/adjust
Content-Type: application/json

{
  "productId": "507f1f77bcf86cd799439011",
  "quantity": 100,
  "reason": "inventory_correction",
  "notes": "Contagem física"
}
```

### Histórico de Movimentações
```http
GET /stock/history/:productId?limit=20&startDate=2026-05-01&endDate=2026-05-06
```

**Response (200)**:
```json
{
  "data": [
    {
      "id": "507f1f77bcf86cd799439013",
      "productId": "507f1f77bcf86cd799439011",
      "type": "IN",
      "quantity": 50,
      "previousQuantity": 45,
      "newQuantity": 95,
      "reason": "purchase",
      "notes": "Pedido #12345",
      "createdAt": "2026-05-06T15:05:00Z"
    }
  ],
  "total": 1
}
```

### Relatório de Estoque
```http
GET /stock/report
```

**Response (200)**:
```json
{
  "summary": {
    "totalProducts": 25,
    "totalQuantity": 500,
    "totalValue": 15000.00,
    "lowStockProducts": 3,
    "outOfStockProducts": 1
  },
  "byCategory": [
    {
      "category": "Periféricos",
      "products": 10,
      "quantity": 250,
      "value": 7500.00
    }
  ],
  "alerts": [
    {
      "productId": "507f1f77bcf86cd799439011",
      "name": "Mouse Óptico",
      "quantity": 5,
      "minStock": 10,
      "status": "LOW_STOCK"
    }
  ]
}
```

---

## 🔔 Alertas

### Listar Alertas
```http
GET /alerts?isRead=false&severity=HIGH
```

**Response (200)**:
```json
{
  "data": [
    {
      "id": "507f1f77bcf86cd799439014",
      "productId": "507f1f77bcf86cd799439011",
      "productName": "Mouse Óptico",
      "type": "LOW_STOCK",
      "severity": "MEDIUM",
      "message": "Estoque baixo: 5 unidades (mínimo: 10)",
      "isRead": false,
      "createdAt": "2026-05-06T15:10:00Z"
    }
  ],
  "total": 1
}
```

### Marcar Alerta como Lido
```http
PUT /alerts/:id/read
```

### Deletar Alerta
```http
DELETE /alerts/:id
```

### Limpar Todos os Alertas
```http
DELETE /alerts/clear?isRead=true
```

---

## 🧪 Exemplos cURL

### Criar um Produto
```bash
curl -X POST http://localhost:5000/api/products \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer seu_token" \
  -d '{
    "name": "Monitor LG 24\"",
    "sku": "MON-LG-24",
    "category": "Monitores",
    "minStock": 5,
    "maxStock": 50,
    "price": 899.90
  }'
```

### Adicionar Estoque
```bash
curl -X POST http://localhost:5000/api/stock/add \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer seu_token" \
  -d '{
    "productId": "507f1f77bcf86cd799439011",
    "quantity": 20,
    "reason": "purchase"
  }'
```

### Listar Alertas
```bash
curl -X GET http://localhost:5000/api/alerts?isRead=false \
  -H "Authorization: Bearer seu_token"
```

---

## ❌ Códigos de Erro

| Status | Mensagem | Descrição |
|--------|----------|-----------|
| 400 | Bad Request | Dados inválidos na requisição |
| 401 | Unauthorized | Token JWT inválido ou ausente |
| 403 | Forbidden | Sem permissão para acessar o recurso |
| 404 | Not Found | Recurso não encontrado |
| 409 | Conflict | SKU duplicado ou violação de constraint |
| 500 | Internal Server Error | Erro no servidor |

## ⚠️ Tipos de Alerta

- **OUT_OF_STOCK**: Produto sem estoque
- **LOW_STOCK**: Estoque abaixo do mínimo
- **HIGH_STOCK**: Estoque acima do máximo
- **REORDER_NEEDED**: Reposição necessária

## 📈 Severidade

- **HIGH**: Crítico (sem estoque)
- **MEDIUM**: Médio (baixo estoque)
- **LOW**: Baixo (outras situações)
