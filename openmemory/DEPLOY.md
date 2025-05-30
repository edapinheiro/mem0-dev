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
3. **Volume mapping removido**: Eliminado o mapeamento que sobrescrevia a imagem
4. **Persistência de dados**: Volume separado para o banco SQLite
5. **Configuração de produção**: Otimizada para ambientes de produção
6. **Porta da UI**: Alterada para 3001 (container e host usam porta 3001)

### Principais correções para o erro "Could not import module 'main'":

- ❌ **Volume mapping removido**: `./api:/usr/src/openmemory` estava sobrescrevendo a imagem
- ✅ **Working directory automático**: Usa o definido no Dockerfile
- ✅ **PYTHONPATH removido**: Não necessário quando o código está no local correto
- ✅ **Build context correto**: Usa a imagem construída, não o código local

### Mapeamento de Variáveis:

- `OPENAI_API_KEY` (Coolify) → `API_KEY` (interno da aplicação)
- `USER` (Coolify) → `USER` (interno da aplicação)
- `NEXT_PUBLIC_API_URL` (Coolify) → `NEXT_PUBLIC_API_URL` (UI)
- `NEXT_PUBLIC_USER_ID` (Coolify) → `NEXT_PUBLIC_USER_ID` (UI)

## Comandos de Deploy

```bash
# Build e deploy (força rebuild da imagem)
docker-compose up --build -d

# Verificar logs
docker-compose logs -f openmemory-mcp

# Parar serviços
docker-compose down
```

## Verificação de Saúde

Após o deploy, verifique:

1. API: `http://seu-dominio:8765/docs`
2. UI: `http://seu-dominio:3001`
3. Qdrant: `http://seu-dominio:6333`

## Troubleshooting

Se ainda houver problemas:

1. Verifique se a OPENAI_API_KEY está configurada no Coolify
2. Force rebuild: `docker-compose build --no-cache`
3. Verifique os logs: `docker-compose logs openmemory-mcp`
4. Confirme que todas as portas estão disponíveis

## Notas Importantes

- **Dados persistem**: O banco SQLite agora é salvo em volume separado
- **Sem desenvolvimento**: Volumes de código removidos para produção
- **Build otimizado**: Imagem usa o código construído, não mapeado
- **Porta da UI**: Container e host usam porta 3001 (mapeamento 3001:3001) 