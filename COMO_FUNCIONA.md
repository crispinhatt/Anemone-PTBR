# Como funciona este projeto

Este documento explica em detalhe o processo técnico usado para localizar e traduzir a DLC *Commander Lilith & the Fight for Sanctuary* de Borderlands 2, do início ao fim.

---

## 1. O problema inicial

Diferente do jogo base, que vem com a pasta `INT` (inglês) completa, a pasta de localização da DLC instalada pela Steam só contém **4 arquivos parcialmente traduzidos** por terceiros:

```
DLC\Anemone\Compat\Localization\INT\
├── Ake_Anemone_VO.int      (falas de voz — ~49% traduzido)
├── Subtitles.int           (cinemáticas — completo)
├── WillowGame.int          (tutoriais — completo)
└── WillowMenu.int          (menu nível 30 — completo)
```

Todo o restante do jogo (nomes de missão, objetivos, inimigos, itens, desafios) ainda aparecia **em inglês ou alemão** durante o gameplay, mesmo após a tradução parcial do VO.

## 2. Localizando a estrutura completa de localização

A Gearbox não disponibilizou uma pasta `INT` completa para a DLC, mas a localização **alemã (`DEU`) está 100% completa** oficialmente. Essa pasta não vem instalada com o jogo por padrão — ela está disponível através dos *depots* da Steam.

### 2.1. Identificando os depots corretos

Usando o [SteamDB](https://steamdb.info/app/49520/depots/), identificamos os depots do jogo base (appid `49520`) que continham a pasta `DEU`:

| Depot    | Conteúdo                                                      |
| -------- | --------------------------------------------------------------- |
| `49521`  | Conteúdo do jogo base (mapas, personagens)                     |
| `49522`  | Executáveis, shaders, engine — sem DLC                         |
| `49523`  | **Localização completa** — todos os arquivos `.deu` de todas as DLCs |
| `49524`  | Áudio do jogo base                                              |
| `213791` | Conteúdo da DLC Anemone (`.upk`, filmes, texturas)              |

### 2.2. Baixando os depots

Usando o [DepotDownloader](https://github.com/SteamRE/DepotDownloader):

```
DepotDownloader.exe -app 49520 -depot 49523 -username SEU_LOGIN -dir "C:\BL2_Download"
```

O caminho resultante contém:
```
depot_49523\DLC\Anemone\Compat\Localization\DEU\
```
— com **182 arquivos `.deu`**, a estrutura completa de localização da DLC.

## 3. Extraindo e estruturando os dados

Um script Python (`scan_anemone.py`) foi usado para:

1. Ler todos os 182 arquivos `.deu`, detectando automaticamente a codificação (UTF-16 LE, a usada pelo Unreal Engine 3).
2. Fazer o parsing da estrutura `[Seção] Chave=Valor` característica dos arquivos `.int`/`.deu`.
3. Identificar campos traduzíveis (`Description`, `MissionName`, `ChallengeName`, `DisplayName`, etc.) e separá-los de campos técnicos/referências internas.
4. Exportar tudo para um único `anemone_localization.json`, com estatísticas de quantas strings cada arquivo continha.

```python
# Exemplo simplificado da lógica de parsing
def parse_loc_file(content):
    sections = {}
    current = "_root"
    for line in content.splitlines():
        if line.startswith("[") and line.endswith("]"):
            current = line[1:-1]
            sections[current] = {}
        elif "=" in line:
            key, _, value = line.partition("=")
            sections[current][key.strip()] = value.strip()
    return sections
```

## 4. Tradução com a CLAUDE

Com o JSON estruturado, o conteúdo foi traduzido em lotes manuseáveis, arquivo por arquivo, mantendo:

- A personalidade de cada personagem (Tiny Tina exagerada e cheia de gírias, Claptrap dramático e autocentrado, Vaughn tentando soar "bandido", Zed com sotaque caipira, Lilith séria e direta).
- Termos próprios do universo Borderlands mantidos em inglês (`Vault Hunter`, `Eridium`, `Slag`, nomes próprios).
- Formatação especial preservada (`\n` para quebras de linha em diálogos, tags como `[skill]...[-skill]` usadas pelo jogo para colorir texto).

Os arquivos `.int` finais foram gerados em UTF-16 LE com BOM, exatamente como o Unreal Engine 3 espera:

```python
def write_int(filename, sections):
    lines = ["\ufeff"]
    for section, kvs in sections.items():
        lines.append(f"[{section}]")
        for k, v in kvs.items():
            lines.append(f"{k}={v}")
        lines.append("")
    content = "\r\n".join(lines)
    with open(filename, "wb") as f:
        f.write(content.encode("utf-16-le"))
```

## 5. O problema do arquivo de falas de voz (VO)

O arquivo `Ake_Anemone_VO.int` que já existia parcialmente traduzido (citado na seção 1) apresentava um problema grave: **a estrutura interna estava desalinhada** em relação ao arquivo original. Isso fazia com que a legenda exibida na tela correspondesse a uma fala diferente da que estava sendo reproduzida no áudio.

Para resolver:

1. Foi obtida uma cópia **limpa e não-modificada** do arquivo `Ake_Anemone_VO.int` em inglês, extraída diretamente de uma instalação nova do jogo (Epic Games Store).
2. Esse arquivo foi usado como **base estrutural definitiva** — preservando a ordem exata das 984 seções e seus timestamps.
3. Um dicionário de tradução EN → PT-BR foi construído cobrindo as 984 falas, reaproveitando as traduções já existentes e completando as que faltavam.
4. A tradução foi aplicada sobre essa base limpa, garantindo que cada legenda corresponda exatamente à fala de áudio certa.

## 6. Varredura final de qualidade

Após a primeira rodada de tradução, uma varredura automatizada teve de ser repetida várias vezes, pois resíduos de texto em alemão continuavam aparecendo em arquivos que pareciam já traduzidos (geralmente em `SkillName`, `Description` de itens, e nomes de inimigos com variações como "Badass", "rasender/furioso", "geschockt/chocado").

A solução foi escrever um script que escaneia **todos os 180 arquivos** simultaneamente, buscando por:
- Caracteres especiais do alemão (`ü`, `ö`, `ä`, `ß`)
- Palavras funcionais comuns (`der`, `die`, `das`, `und`, `wenn`, `nicht`, `für`)
- Padrões de substantivos técnicos do jogo em alemão (`Schild-Skill`, `Schaden`, `Gegenstand`)

```python
german_words = ['ü', 'ö', 'ä', 'ß', ' und ', ' der ', ' die ', ' das ',
                ' wenn ', '-Skill"', 'Schild-', 'Gegenstand']

for fname in arquivos:
    for linha in arquivo:
        if any(palavra in linha for palavra in german_words):
            reportar(fname, linha)
```

Cada ocorrência real foi corrigida manualmente; falsos positivos (palavras em português que continham substrings coincidentes, como "poder" contendo "der") foram verificados individualmente antes de serem ignorados.

## 7. Estrutura final

```
180 arquivos .int
├── 1 arquivo de falas de voz (984 entradas)
└── 179 arquivos de missões, inimigos, itens, interface, etc.
```

Todos em UTF-16 LE, prontos para serem colocados diretamente na pasta `Localization\INT` da DLC.

---

## Replicando este processo para outra DLC ou idioma

Se você quiser aplicar esse mesmo processo a outra DLC de Borderlands 2 (ou outro jogo que use o engine Unreal 3 com o mesmo sistema `.int`):

1. Verifique se existe uma pasta de localização completa em outro idioma (alemão, francês, espanhol, etc.) — geralmente a Gearbox traduziu tudo para os principais idiomas europeus.
2. Use o `DepotDownloader` para baixar o depot correto (verifique no SteamDB qual depot contém a pasta `Localization`).
3. Adapte o script de parsing (`scan_anemone.py`) para o nome correto da DLC.
4. Traduza os campos extraídos usando a CLAUDE ou outra ferramenta de tradução com contexto de personagem.
5. Sempre valide o arquivo de falas de voz contra uma cópia limpa do original em inglês, para evitar desalinhamentos de estrutura.
6. Rode uma varredura final em todos os arquivos gerados antes de publicar.
