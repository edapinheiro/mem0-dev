# Instruções de Deploy - OpenMemory

## Configuração de Variáveis de Ambiente

As variáveis de ambiente agora estão alinhadas com o Coolify:

### Variáveis necessárias no Coolify:

- `OPENAI_API_KEY`: Sua chave da OpenAI (obrigatória)
- `USER`: ID do usuário (ex: default_user)
- `NEXT_PUBLIC_API_URL`: URL da API (ex: https://sua-api.coolify.com)
- `NEXT_PUBLIC_USER_ID`: ID do usuário para a UI (pode ser o mesmo que USER)

### Exemplo de configuração local (.env):

```env
# Required: OpenAI API Key
OPENAI_API_KEY=sua_chave_openai_aqui

# User Configuration
USER=default_user
NEXT_PUBLIC_USER_ID=default_user

# API URL para a UI
NEXT_PUBLIC_API_URL=http://localhost:8765
```

## Deploy

O `docker-compose.yml` foi corrigido para resolver os seguintes problemas:

1. **Comando uvicorn malformado**: Agora usa array format
2. **Variáveis de ambiente**: Alinhadas com as do Coolify
3. **Working directory**: Alinhado com o Dockerfile
4. **Dependências**: Configuradas corretamente

### Mapeamento de Variáveis:

- `OPENAI_API_KEY` (Coolify) → `API_KEY` (interno da aplicação)
- `USER` (Coolify) → `USER` (interno da aplicação)
- `NEXT_PUBLIC_API_URL` (Coolify) → `NEXT_PUBLIC_API_URL` (UI)
- `NEXT_PUBLIC_USER_ID` (Coolify) → `NEXT_PUBLIC_USER_ID` (UI)

## Comandos de Deploy

```bash
# Build e deploy
docker-compose up --build -d

# Verificar logs
docker-compose logs -f openmemory-mcp

# Parar serviços
docker-compose down
```

## Verificação de Saúde

Após o deploy, verifique:

1. API: `http://seu-dominio:8765/docs`
2. UI: `http://seu-dominio:3000`
3. Qdrant: `http://seu-dominio:6333`

## Troubleshooting

Se ainda houver problemas:

1. Verifique se a OPENAI_API_KEY está configurada no Coolify
2. Confirme que todas as portas estão disponíveis
3. Verifique os logs: `docker-compose logs openmemory-mcp` 