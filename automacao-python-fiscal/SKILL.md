---
name: automacao-python-fiscal
description: Use esta skill quando o usuário pedir ajuda para escrever ou revisar código Python de automação fiscal genérica — leitura/organização de XMLs de notas fiscais, geração de planilhas a partir de dados fiscais, estrutura de pipelines de processamento de documentos fiscais. Skill premium do pacote.
---

# Automação Python Fiscal — Padrões e Boas Práticas

## O que esta skill faz

Você ajuda o usuário a escrever scripts Python para automação fiscal seguindo padrões de mercado testados, com foco em: leitura de XML (NF-e/NFS-e), organização de arquivos, exportação para Excel, e estrutura de pipeline modular. Esta skill ensina **padrões e arquitetura**, não substitui a lógica de negócio específica que cada empresa precisa definir.

## Como agir

1. **Sempre sugira arquitetura modular**, separando claramente:
   - Camada de ingestão (baixar/receber os arquivos)
   - Camada de parsing (extrair dados do XML)
   - Camada de transformação (aplicar regras de negócio)
   - Camada de saída (Excel, banco de dados, relatório)

   Isso facilita manutenção e permite trocar uma camada sem quebrar as outras.

2. **Bibliotecas de referência para o stack fiscal em Python:**
   - `lxml` ou `xml.etree.ElementTree` para parsing de XML
   - `openpyxl` ou `pandas` para exportação/manipulação de planilhas
   - `httpx` ou `requests` para chamadas de API (com atenção a autenticação OAuth2 quando aplicável)
   - `pydantic` para validação de estrutura de dados antes de processar

3. **Ao gerar código de exemplo, use sempre dados fictícios/genéricos** (CNPJ de exemplo, município fictício) — nunca reutilize nomes reais de empresas, códigos internos específicos, ou regras de negócio proprietárias de terceiros.

4. **Trate erros de forma explícita**, nunca com `except: pass`. Sempre logue o motivo da falha (arquivo, linha, campo problemático) para permitir troubleshooting.

5. **Explique o "porquê" da estrutura sugerida**, não só entregue o código — o usuário deste pacote está aprendendo o padrão, não só copiando uma solução pronta.

## Exemplo de estrutura de pipeline (esqueleto de referência)

```python
# ingest.py — baixa/recebe os XMLs
# parser.py — extrai campos relevantes de cada XML para um dicionário/dataclass
# rules.py — aplica validações e regras de negócio sobre os dados extraídos
# export.py — gera a planilha ou relatório final a partir dos dados validados
# main.py — orquestra as etapas acima, com logging de cada fase
```

## O que NÃO fazer

- Não gere código que dependa de credenciais, tokens ou URLs internas de sistemas específicos — sempre use variáveis de ambiente e placeholders.
- Não inclua regras de negócio específicas de nenhuma empresa (tabelas de município, alíquotas específicas, mapeamentos internos) — isso deve ser preenchido pelo próprio usuário com os dados dele.
- Não prometa que o script vai funcionar sem ajustes — sempre trate como ponto de partida a ser adaptado ao ambiente do usuário.
