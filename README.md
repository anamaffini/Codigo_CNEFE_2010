# README — CNEFE 2010 → Faces de Quadra

## Objetivo
Este notebook tem como finalidade **espacializar e categorizar os dados do CNEFE 2010 (Cadastro Nacional de Endereços para Fins Estatísticos)** a partir dos arquivos em formato **TXT de largura fixa**.  

Ele realiza:
1. **Leitura do TXT** conforme as especificações oficiais de colunas (fixed width).
2. **Construção das chaves geográficas** (município, setor, face de quadra).
3. **Classificação dos usos**:
   - Pelo código `especie_uso` (1–7).
   - Refinamento do código **6 = Outros estabelecimentos** com base na descrição do estabelecimento (`identiEstab`).
4. **Agregação por face de quadra** → gera tabelas com contagens de estabelecimentos por categoria.
5. **Integração com shapefile** de faces do IBGE (2010), garantindo correspondência entre TXT e SHP mesmo quando os códigos possuem tamanhos diferentes.
6. **Exportação** dos resultados em formatos geoespaciais (GeoPackage) e tabulares (CSV).

---

## Entradas
- **TXT** do CNEFE (ex.: `43000340500.TXT`): arquivo de largura fixa com informações de endereços e estabelecimentos.  
- **SHP** de faces de quadra (ex.: `43000340500_face.shp` e arquivos auxiliares `.dbf`, `.shx`, `.prj`): base espacial do IBGE.

---

## Passos principais
1. **Leitura via `pd.read_fwf`** usando `colspecs` e `column_names` predefinidos.
2. **Preenchimento com zeros** para padronizar os códigos (UF, município, distrito, subdistrito, setor, quadra, face).
3. **Construção das chaves**:
   - `ID_municipio` (7 dígitos: UF + Município).
   - `ID_setor` (15 dígitos: UF + Município + Distrito + Subdistrito + Setor).
   - `ID_face` (21 dígitos: UF + Município + Distrito + Subdistrito + Setor + Quadra + Face).
4. **Classificação dos usos**:
   - `especie_uso` mapeado para categorias principais (`Residencial`, `Ensino`, `Saúde`, etc.).
   - `Outros estabelecimentos` refinados pela descrição em `identiEstab` (palavras-chave como *mercado*, *padaria*, *bar*, *oficina*, etc.).
5. **Agregação por face**: contagem total e detalhada por categoria.
6. **Join com o shapefile**:
   - O notebook testa automaticamente várias colunas (`CD_GEO`, `CD_GEOCODI`, `FACE`, etc.) e diferentes comprimentos de sufixo (14, 15, 21, etc.).
   - Seleciona a combinação que gera a maior taxa de correspondência.
7. **Exportação dos resultados**:
   - `cnefe_faces_fwf.gpkg` → arquivo geoespacial.
   - `cnefe_por_face_fwf.csv` → tabela agregada por face.

---

## Saídas
- `saida_fwf/cnefe_faces_fwf.gpkg` → camada geográfica com atributos agregados.  
- `saida_fwf/cnefe_por_face_fwf.csv` → tabela em formato CSV com as contagens por categoria.  

---

## Como usar
1. Ajuste no início do notebook os caminhos para:
   ```python
   TXT_PATH = Path("caminho/para/43000340500.TXT")
   FACE_SHP = Path("caminho/para/43000340500_face.shp")
   ```
2. Execute todas as células na ordem.
3. Verifique no console/log:
   - **Quantidade de matches** entre `ID_face` do TXT e do SHP.
   - Arquivos exportados em `OUT_DIR`.

---

## ⚠️ Observações importantes
- Certifique-se de ter **todos os arquivos do shapefile** (`.shp`, `.shx`, `.dbf`, `.prj`).
- Se a taxa de match for **zero**, identifique no SHP qual coluna contém o geocódigo de face (ex.: `CD_GEOCODI`) e ajuste manualmente no notebook.
- Os dicionários de categorias (`MAP_CNEFE` e `SUBMAP_OUTROS`) podem ser expandidos ou modificados conforme necessário.
