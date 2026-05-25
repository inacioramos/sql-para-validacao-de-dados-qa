# 🗄️ SQL aplicado a Quality Assurance (QA) e Validação de Dados

Este repositório reúne cenários práticos e scripts SQL estruturados para validação de dados, testes de backend (caixa branca) e garantia de integridade em bancos de dados relacionais. 

O objetivo deste projeto é demonstrar a aplicação de queries SQL no dia a dia de um Analista de QA para identificar inconsistências que não são visíveis na interface gráfica (UI).

---

## 🔍 Cenários de Teste e Queries Aplicadas

Os cenários abaixo simulam validações essenciais de regras de negócio, integridade referencial e consistência de dados em um ambiente de automação comercial / ERP.

### 1. Validação de Regra de Negócio: Prevenção de Duplicidade
* **Objetivo do Teste:** Garantir que o sistema e o banco de dados estão respeitando a restrição de unicidade (Unique Key), impedindo a duplicidade de cadastros sensíveis (como CPF/CNPJ de clientes ou código de barras de produtos).
* **Query de Validação:**
```sql
SELECT codigo_barra, COUNT(*) 
FROM produtos 
GROUP BY codigo_barra 
HAVING COUNT(*) > 1;
```
### 2. Validação de Relatórios: Consistência Financeira
* **Objetivo do Teste:** Validar se o somatório de vendas exibido nos relatórios da interface do sistema bate exatamente com os valores reais consolidados diretamente nas tabelas do banco de dados.
* **Query de Validação:**
```sql
SELECT CAST(data_venda AS DATE) AS data_dia, SUM(valor_total) AS total_faturado
FROM vendas
WHERE status = 'CONCLUIDA'
GROUP BY CAST(data_venda AS DATE)
ORDER BY data_dia DESC;
```
### 3. Teste de Campos Obrigatórios (Dados Nulos ou Inválidos)
* **Objetivo do Teste:** Verificar se o sistema permitiu a gravação de registros "órfãos" ou com campos obrigatórios em branco (Nulos), o que quebraria as regras de negócio.

* **Query de Validação:**
```sql
SELECT id_venda, cliente_id, valor_total 
FROM vendas 
WHERE cliente_id IS NULL OR valor_total <= 0;
```

