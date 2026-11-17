# GeoAnalyzer Pro — Análise espacial no navegador

**GeoAnalyzer Pro** (v2.5) é uma aplicação web de análise geoespacial que roda
inteiramente no navegador, em um único arquivo HTML. Ela cruza **shapefiles**
(polígonos de áreas de interesse) com **rasters GeoTIFF** classificados (uso e
cobertura da terra, cicatrizes de fogo, etc.), recorta cada raster pelas feições
de cada shapefile e calcula, por classe, a **área em hectares** e a **moda**
(classe dominante). Os resultados aparecem em tabela, gráficos e mapa interativo
e podem ser exportados para Excel (XLSX), CSV, PNG e SVG.

O código-fonte da aplicação está na pasta [`GeoAnalyzerPro/`](.).

---

## O que o app faz

| Recurso | Descrição |
|---|---|
| Leitura de shapefiles | Aceita `.zip` (com `.shp`, `.dbf`, `.prj`, `.shx`) ou os arquivos avulsos. Lê **todos** os campos de atributos direto do cabeçalho do `.dbf`. |
| Leitura de rasters | Aceita `.tif`, `.tiff`, `.geotiff`. Detecta o sistema de coordenadas (EPSG, geográfico ou projetado), o valor *NoData* e todos os valores de classe presentes no raster. |
| Reprojeção automática | Identifica o CRS de cada camada (via `.prj`/GeoKeys) e alinha a geometria ao CRS do raster antes do recorte (proj4js; zona UTM inferida quando necessário). |
| Dicionário de classes | Associa cada valor de pixel a um rótulo (ex.: `4 → Savana`). O dicionário pode ser editado, salvo em TXT/CSV e importado (separadores `,` `;` tabulação ou `|`). |
| Processamento | Para cada combinação *shapefile × raster*, recorta o raster por feição e calcula a área por classe (ha) e a moda, com barra de progresso, progresso por arquivo e log. |
| Tabela de dados | Resultados paginados (10/25/50/100), ordenáveis, com filtros por shape, raster, área e classe, e indicadores (área total, moda dominante, nº de shapes). |
| Insights | Gráficos: pizza por classe, barras por classe, Top 15 features, área acumulada, frequência da moda e composição 100% empilhada. Cada gráfico exporta em PNG, SVG e/ou CSV. |
| Mapa | Mapa base com as camadas carregadas, rasters recortados com legenda, rótulos das feições e estatísticas por feição ao clicar. |
| Relatórios | Exportação XLSX analítica (todos os dados e estatísticas) ou somente dos dados filtrados; log de operações exportável. |

Nenhum dado é enviado a servidor: todo o processamento ocorre localmente no
navegador.

---

## Requisitos

- Navegador moderno atualizado (Chrome, Edge ou Firefox).
- Conexão com a internet no primeiro uso, para carregar as bibliotecas
  (Leaflet, shpjs, geotiff.js, proj4js, html2canvas) a partir de CDN e o mapa base.
- Não há instalação nem dependências locais.

---

## Tutorial de uso

### 1. Abrir o aplicativo

1. Baixe o repositório (**Code → Download ZIP**) ou clone-o:
   ```bash
   git clone https://github.com/rnvuefsppgm/GeoFireAnalyzer.git
   ```
2. Abra o arquivo `GeoAnalyzerPro/GeoAnalyzer_v2.html` (nesta pasta) com um duplo clique.
3. A barra lateral mostra o fluxo de trabalho: **Arquivos → Processamento →
   Tabela de dados → Insights → Mapa**. O botão **Recolher** encolhe a barra e
   **Novo projeto** limpa tudo para começar de novo.

### 2. Carregar os arquivos (página *Arquivos*)

1. Em **Shapefiles**, defina o **Nº Shapes** e clique em **Atualizar**, ou use
   **+ Adicionar Shapefile**.
2. Em cada slot, selecione o `.zip` do shapefile (recomendado) ou os arquivos
   `.shp`, `.dbf`, `.prj` e `.shx` juntos.
3. Escolha o **atributo** que identifica cada feição (ex.: nome do município,
   ID do imóvel). Opcionalmente renomeie o shape.
4. Em **Rasters**, repita o procedimento com **Nº Rasters** / **+ Adicionar
   Raster** e selecione os GeoTIFF.
5. Após a leitura, o app lista as classes encontradas no raster. Preencha o
   **dicionário de classes** com os rótulos (ex.: `3 = Formação Florestal`), ou
   importe um TXT/CSV no formato `valor,rótulo`, um por linha. Use **salvar**
   para reaproveitar o dicionário em outros projetos.
6. O painel **Uso de armazenamento** mostra quanto memória os shapefiles e
   rasters estão ocupando.

> Dica: shapefiles e rasters podem estar em sistemas de coordenadas diferentes;
> o app reprojeta automaticamente. Mantenha o `.prj` junto do shapefile para a
> detecção ser precisa.

### 3. Processar (página *Processamento*)

1. Clique em **Gerar insights espaciais** (em *Arquivos*) ou em **Iniciar
   Processamento**.
2. Acompanhe o **Progresso Geral** (shapes, rasters, combinações e concluídas),
   o **Progresso por Arquivo** e o **Processing Log**.
3. Ao final aparece **✅ Processamento Concluído**, com atalhos para **Ver
   tabela**, **Ver insights** e **Ver mapa**.
4. **Limpar Resultados** descarta os resultados para reprocessar.

### 4. Consultar a tabela (página *Tabela de dados*)

- Colunas: **Nome do Shape**, **Atributo**, **Área HA**, **Moda** e **Nome do
  Raster**. Clique no cabeçalho para ordenar e na linha para ver o detalhe por
  classe.
- Filtre por shape e raster e ajuste a quantidade de linhas por página.
- **Filtros Avançados**: área mínima/máxima (ha) e classe mínima/máxima.
- **Exportar XLSX** exporta a tabela filtrada; **Gerar XLSX analítico** gera o
  relatório completo com todos os dados e estatísticas.

### 5. Analisar os gráficos (página *Insights*)

1. Selecione o shape e o raster (ou *Todos*) e clique em **Aplicar**.
2. Indicadores: área total analisada, classes identificadas, rasters analisados
   e shapes processados.
3. Gráficos disponíveis:
   - **Distribuição por Classe** (pizza, área total em ha por classe);
   - **Área por Classe** (barras, comparativo em ha);
   - **Área por Feature (Top 15)** (barras horizontais);
   - **Área Acumulada** (progressiva por feature);
   - **Frequência da Moda** (classe dominante por feature);
   - **Composição por Feature** (barras empilhadas 100%).
4. Use os botões **PNG**, **SVG** ou **CSV** de cada gráfico para exportá-lo.
5. A tabela **Últimas operações** registra cada operação; use **Exportar log**.

### 6. Explorar o mapa (página *Mapa*)

- Escolha o shape a exibir, ative **Visualizar rasters** para ver os rasters
  recortados e use **Enquadrar** e **Rótulos**.
- Em **Rasters recortados**, ligue/desligue cada camada; a **Legenda** usa os
  rótulos do dicionário.
- Clique em uma feição para ver os **atributos** e as **estatísticas da
  seleção** por raster.

---

## Exemplo de aplicação

Quantificar a área queimada por classe de uso do solo em cada município:
carregue o shapefile de municípios, o raster de cicatrizes de fogo e o raster de
uso e cobertura (ex.: MapBiomas), preencha o dicionário de classes, processe e
exporte o XLSX analítico com as áreas em hectares por município e classe.

---

## Estrutura do repositório

```
GeoAnalyzerPro/
├── GeoAnalyzer_v2.html   # aplicação completa (HTML + CSS + JavaScript)
├── README.md             # este tutorial
└── LICENSE               # GPL-3.0
LICENSE                   # GPL-3.0
README.md                 # este tutorial
```

---

## Licença

Este programa é software livre, distribuído sob a **GNU General Public License
v3.0 (GPL-3.0)**. Você pode redistribuí-lo e/ou modificá-lo nos termos dessa
licença. O texto completo está no arquivo [`LICENSE`](LICENSE).

Bibliotecas de terceiros usadas pelo app mantêm suas próprias licenças:
Leaflet (BSD-2-Clause), shpjs (MIT), geotiff.js (MIT), proj4js (MIT) e
html2canvas (MIT).
