# GestaoELMO - Sistema de Gestão de Estoque

Sistema completo de gerenciamento de estoque com rastreamento, gestão de produtos e notificações automáticas de estoque baixo.

## 🎯 Funcionalidades

- ✅ **Rastreamento de Estoque**: Monitore produtos em tempo real
- ✅ **Gestão de Produtos**: Cadastro e atualização de produtos
- ✅ **Alertas de Baixo Estoque**: Notificações automáticas quando estoque atinge limite mínimo
- ✅ **Notificação de Ações**: Registro de todas as operações de estoque
- ✅ **Relatórios**: Análise de movimentação de estoque
- ✅ **Dashboard**: Interface visual para monitoramento

## 🛠️ Tecnologias

- **Backend**: Node.js + Express
- **Banco de Dados**: MongoDB
- **Frontend**: React + TypeScript
- **Notificações**: Socket.io + Email
- **Logging**: Winston

## 📁 Estrutura do Projeto

```
GestaoELMO/
├── backend/
│   ├── src/
│   │   ├── models/
│   │   ├── controllers/
│   │   ├── services/
│   │   ├── routes/
│   │   ├── middleware/
│   │   ├── utils/
│   │   └── app.ts
│   ├── tests/
│   └── package.json
├── frontend/
│   ├── src/
│   │   ├── components/
│   │   ├── pages/
│   │   ├── services/
│   │   ├── hooks/
│   │   └── App.tsx
│   └── package.json
├── docker-compose.yml
└── README.md
```

## 🚀 Começando

### Pré-requisitos
- Node.js v16+
- MongoDB
- npm ou yarn

### Instalação

1. Clone o repositório
```bash
git clone https://github.com/douglaslms00/GestaoELMO.git
cd GestaoELMO
```

2. Instale as dependências do backend
```bash
cd backend
npm install
```

3. Instale as dependências do frontend
```bash
cd ../frontend
npm install
```

4. Configure as variáveis de ambiente
```bash
cp .env.example .env
```

5. Inicie os serviços
```bash
docker-compose up
```

## 📚 Documentação da API

Veja a documentação completa em [API.md](./docs/API.md)

## 🔔 Notificações

O sistema suporta múltiplos canais de notificação:
- Email
- WebSocket (Real-time)
- SMS (Opcional)

## 📄 Licença

MIT
