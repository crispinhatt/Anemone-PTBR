# Borderlands 2: Commander Lilith & the Fight for Sanctuary — Tradução PT-BR

> Tradução não-oficial completa da DLC para o Português do Brasil.

---

## 📋 Sobre o projeto

Este projeto assim como sua documentação foram feitos inteiramente com o uso da plataforma CLAUDE, sendo apenas alguns poucos termos corrigidos manualmente.

Este projeto traduz **984 falas de voz** e **mais de 600 textos de interface** da DLC *Commander Lilith & the Fight for Sanctuary* de Borderlands 2 para o Português do Brasil, cobrindo todos os textos visíveis do jogo:

- Falas de voz de todos os personagens (Vault Hunter, Lilith, Mordecai, Brick, Tiny Tina, Claptrap, Vaughn, Cassius, Tannis, Hector, etc.)
- Missões principais e secundárias (objetivos, descrições, resumos)
- Cinemáticas de abertura e encerramento
- Inimigos (infectados, soldados da Nova Pandora, Loaders hackeados, fauna)
- Itens, armas, escudos, relíquias e granadas exclusivos da DLC
- Desafios e desafios de nível
- Ícones de interação (botões de ação no mundo)
- Tutoriais e avisos (spoiler, imunidade ao gás)
- Tela de criação de personagem nível 30

**Versão do jogo testada:** Build mais recente disponível na Steam/Epic Games (Jun/2026)
**Plataforma:** PC (Steam / Epic Games Store)

---

## ✅ O que foi traduzido

| Categoria                       | Arquivos | Descrição                                          |
| -------------------------------- | -------- | --------------------------------------------------- |
| Ake_Anemone_VO                  | 984      | Falas de voz de todos os personagens                |
| Missões principais (Plot)       | 8        | As 6 missões da campanha principal                  |
| Missões secundárias (Side)      | 13       | Todas as missões secundárias da DLC                 |
| Inimigos e população            | ~70      | Infectados, soldados, Loaders, fauna, NPCs          |
| Itens / Armas / Equipamentos    | ~15      | Escudos, relíquias, granadas, rifles, armas únicas  |
| Desafios                        | 2        | Desafios gerais e desafios de nível por mapa        |
| Interface e ícones de interação | 4        | Ícones de ação, fast travel, créditos                |
| Cinemáticas e legendas          | 3        | Subtitles, letreiros de personagens                 |
| Diversos (FX, switches, portas) | ~60      | Elementos de cena, efeitos e objetos interativos     |

**Total: 180 arquivos `.int`**

### Termos mantidos em inglês (intencionalmente)

Termos próprios do universo Borderlands e nomes de itens/lugares foram preservados no idioma original quando já consagrados pela comunidade brasileira: `Vault Hunter`, `Vault Key`, `Eridium`, `Slag`, `Sanctuary`, `Helios`, `Skag`, `Rakk`, `Catch-A-Ride`, `BAR` (Badass Rank), nomes próprios de personagens e locais.

---

## Como foi traduzido?

Para saber mais detalhes leia `COMO_FUNCIONA.md`

Com ajuda da CLAUDE foi identificado que a DLC não possuía pasta de localização `INT` original — apenas 4 arquivos haviam sido traduzidos parcialmente por terceiros. A estrutura completa de localização (182 arquivos) só existia na pasta `DEU` (alemão), incluída oficialmente pela Gearbox.

O processo seguiu estas etapas:
1. Download dos depots da Steam (`depot_49523`) contendo a pasta `DEU` completa da DLC.
2. Extração e parsing de todos os arquivos `.deu` para um único `.json` estruturado.
3. Tradução do alemão/inglês para PT-BR usando a CLAUDE, mantendo a personalidade de cada personagem (Tiny Tina exagerada, Claptrap dramático, Zed caipira, etc.).
4. Para o arquivo de falas de voz (`Ake_Anemone_VO.int`), foi necessário comparar com uma cópia limpa do arquivo original em inglês (Epic Games) para corrigir um desalinhamento estrutural that fazia as legendas não baterem com o áudio.
5. Varredura sistemática de todos os 180 arquivos `.int` gerados em busca de resíduos de texto em alemão que escaparam da tradução automática.

Este projeto utilizou a plataforma CLAUDE (claude.ai) para realizar a tradução dos termos.

## 🛠️ Como instalar

### Pré-requisitos

- Borderlands 2 instalado (Steam ou Epic Games Store)
- DLC "Commander Lilith & the Fight for Sanctuary" instalada e atualizada

### Passo a passo

**1. Localize a pasta de localização da DLC**

```
Steam:
...\SteamLibrary\steamapps\common\Borderlands 2\DLC\Anemone\Compat\Localization\INT

Epic Games:
...\Epic Games\Borderlands2\DLC\Anemone\Compat\Localization\INT
```

**2. Faça backup da pasta INT (se já existir)**

Copie a pasta `INT` inteira para um local seguro antes de continuar. Se a pasta não existir, pode pular esta etapa.

**3. Baixe o arquivo de tradução**

Baixe o `Anemone_Localization_INT_PTBR.zip` da seção [Releases](../../releases) deste repositório.

**4. Extraia os arquivos**

Extraia todo o conteúdo do ZIP diretamente para a pasta `INT` mencionada no passo 1. Se solicitado, substitua os arquivos existentes.

**5. Jogue!**

Abra o jogo normalmente pela Steam ou Epic Games em **inglês**. Os textos da DLC aparecerão automaticamente em **Português do Brasil**.

---

## 🔄 Como desinstalar / restaurar

Para voltar ao inglês original:

1. Feche o jogo
2. Delete os arquivos `.int` que foram adicionados pela tradução
3. Restaure o backup da pasta `INT` original (se você tinha um)

---

## ⚠️ Avisos

- Esta é uma tradução **não-oficial** — não tem relação com a Gearbox Software ou 2K Games
- Testada com a build mais recente disponível em Jun/2026 — pode ser necessário reaplicar após atualizações do jogo
- Se o jogo atualizar e a tradução parar de funcionar, restaure o backup e aguarde uma atualização deste projeto
- Algumas falas com gírias e expressões idiomáticas foram adaptadas livremente para manter o tom de humor original em PT-BR, em vez de tradução literal

---

## 🐛 Reportar erros

Encontrou um erro de tradução, legenda que não bate com o áudio, ou algo que não faz sentido em PT-BR? Abra uma [Issue](../../issues) com:

- Print da tela ou nome da missão/personagem
- Contexto (qual missão, menu, momento do diálogo)
- Sugestão de tradução correta (opcional)

---

## 🔧 Para desenvolvedores

Quer contribuir com melhorias ou criar uma tradução para outro idioma? Veja o arquivo [COMO_FUNCIONA.md](COMO_FUNCIONA.md) para entender a estrutura técnica do projeto.

---

## 📜 Licença

Este projeto está licenciado sob a MIT License.

Você pode usar, modificar, distribuir e até utilizar comercialmente este projeto livremente.

Borderlands 2 © Gearbox Software. Publicado por 2K Games.
Este projeto é uma tradução não-oficial e não possui vínculo com os detentores da propriedade intelectual.
