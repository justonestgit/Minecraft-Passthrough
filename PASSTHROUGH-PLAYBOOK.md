# PLAYBOOK — Passthrough Minecraft ↔ Game X

> **Este documento é um procedimento operacional, não um artigo.** Você (agente) vai seguir as fases em
> ordem, passar pelos gates, e no fim terá um passthrough funcionando no jogo real.
>
> **Uso previsto:** o usuário invoca `@mashup-mods` com este arquivo e pede "passthrough de Minecraft
> com <jogo>". A partir daí tudo aqui é executável.

---

## Índice

**Fase 0 — Recon (obrigatório, antes de qualquer código)** → [§0](#fase-0--recon-obrigatório)
**Fase 1 — Arquitetura (decidir, não codar)** → [§1](#fase-1--arquitetura)

**Fase 2 — O adaptador (o trabalho real, por engine)** → [§2](#fase-2--camada-5-o-adaptador)
- [2.1 Unity Mono](#21-unity-mono--bepinex-5--melonloader)
- [2.2 Unity IL2CPP](#22-unity-il2cpp--bepinex-6)
- [2.3 Skyrim / Bethesda](#23-skyrim--bethesda--skse)
- [2.4 Unreal](#24-unreal--ue4ss--ce4ss)
- [2.5 Capcom RE Engine](#25-capcom-re-engine--reframework)
- [2.6 FromSoftware](#26-fromsoftware--modengine2--me3)
- [2.7 RAGE / GTA / RDR](#27-rage--gta--rdr--scripthookv)
- [2.8 Source 1 / Source 2](#28-source-1--source-2)
- [2.9 .NET / XNA](#29-net--xna--tmodloader--smapi)
- [2.10 Godot](#210-godot)
- [2.11 Nativo sem loader](#211-nativo-sem-loader--o-caminho-difícil)
- [2.12 Outro jogo JVM](#212-outro-jogo-jvm)
- [2.13 HTML5 / Electron / NW.js / LÖVE](#213-html5--electron--nwjs--löve)
- [2.14 Engines de dados (responda NÃO)](#214-engines-de-dados--responda-não)

**Fases 3–8 — Camadas reutilizáveis (copie, não reescreva)** → [§3](#fase-3--camada-1--core)
**Fase 9 — Ordem de construção (12 passos com gates)** → [§9](#fase-9--ordem-de-construção)
**Fase 10 — Oracles** → [§10](#fase-10--oracles-obrigatório-por-passo)
**Armadilhas consolidadas** → [§11](#11-armadilhas-consolidadas)
**Checklist por jogo** → [§12](#12-checklist-por-jogo-preenchível)
**Apêndices** → [§13](#apêndice-a--números-de-referência) · [§14](#apêndice-b--as-três-formas-de-dar-profundidade) · [§16](#apêndice-d--referências)

**Legenda:** ✅ verificado em código decompilado ou binário · 📚 conhecimento geral de engenharia ·
⚠️ armadilha ou resultado negativo documentado

---

## Mapa de referência rápida

Três implementações que **existem e funcionam** — seus números e armadilhas são a base disto:

| | Host | Transporte | Render | Autor |
|---|---|---|---|---|
| **MinecraftInsideULTRAKILL 0.2.0** | ULTRAKILL (Unity Mono, BepInEx+Harmony) | WebSocket JSON + shm 284 MB | **β** depth-mesh | chavi |
| **Minecraft dentro do GTA V** | GTA V (RAGE, ScriptHookV + ReShade) | WebSocket JSON + shm | **γ** shader | rehan |
| **SkyCraft 0.1.0** | Skyrim SE (SKSE nativo, DX11, Havok) | **shm 200 MB binário, sem JSON** | **δ** convidado desenha | chasmlol |
| **Portalcraft 0.1.0** | Portal 2 (Source 1, D3D9, **32-bit**) | shm + **previsão de câmera** | **γ** shader | chasmlol |
| **Terrário (FalArsenal)** | Terraria (tModLoader) | — | mod de dados | rehan |

⚠️ **Este playbook cobre um só padrão: dois processos vivos, mods em cada, IPC entre eles.**
As três implementações de referência (acima) fazem exatamente isso e nada mais.

Se a ideia for "traga **uma coisa** de X para Y" — uma arma, um inimigo, um bloco — isso é **porte de
conteúdo**: um mod comum no jogo Y, sem IPC, sem dois processos. Sie meio tempo e não precisa
deste documento.

Passthrough é para quando **a interação física entre os dois é o produto** — o carro batendo
no muro que você construiu, o inimigo de Y atirando no Steve que está no chão de X.

**Se o plugin `mashup-mods` oferecer um padrão mais leve que resolva a ideia, ele está certo e este
playbook não se aplica.** Diga isso ao usuário e pare.

---

# FASE 0 — RECON (obrigatório)

> **Não escreva uma linha de código antes de terminar esta fase.** As três implementações que
> funcionaram gastaram a maior parte do esforço aqui. Uma delas (Wukong) gastou **2 horas de
> recon** para descobrir que o plano inteiro era inviável — e isso foi o resultado mais barato do projeto.

## 0.1 A base de conhecimento primeiro (30 s, economiza dias)

Se você tem o `universal-modder` disponível:

```bash
um kb search "<jogo>"          # este jogo já foi feito?
um kb search "<engine>"        # esta engine já foi vencida?
```

Sem `um`, leia o índice:
`https://github.com/rehan-remade/universal-modder/blob/main/knowledge/INDEX.md`

**Se já existe uma nota:** comece pelas versões exatas, rota e armadilhas dela. Não repita o beco sem
saída de outro agente.

## 0.2 Identificar engine, loader e anti-cheat

```bash
um scan "<jogo>"      # engine+versão, managed ou native, anti-cheat, saves, loaders, rota
```

Se não tiver `um`, identifique manualmente pela tabela de [§2](#fase-2--camada-5-o-adaptador):

| O que você vê no install | Engine |
|---|---|
| `UnityPlayer.dll` + `<Game>_Data/` + `Managed/Assembly-CSharp.dll` | Unity **Mono** → §2.1 |
| `UnityPlayer.dll` + `GameAssembly.dll` + `il2cpp_data/Metadata/global-metadata.dat` | Unity **IL2CPP** → §2.2 |
| `Data/*.esm` + `SkyrimSE.exe` / `Fallout4.exe` + `Data/SKSE/` | Creation Engine → §2.3 |
| `Binaries/Win64/<Proj>-Win64-Shipping.exe` + `Content/Paks/` | Unreal → §2.4 |
| `dinput8.dll` + `reframework/` já presente | RE Engine → §2.5 |
| `GameAssembly.dll` + `me3.exe` / ModEngine2 | FromSoftware → §2.6 |
| `GTA5.exe` + ScriptHookV | RAGE → §2.7 |
| `gameinfo.txt` + `*_dir.vpk` / `engine2.dll` | Source → §2.8 |
| `<Game>.exe` + `<Game>.dll` em `data_*` ou `Managed` (sem Unity) | .NET/XNA → §2.9 |
| `.pck` ao lado do exe ou anexado | Godot → §2.10 |
| Nada reconhecível; C/C++ com `bin/engine.dll` ou exe custom | **nativo** → §2.11 |
| `.jar` principal, sem C++ | JVM → §2.12 |
| `resources/app.asar`, `package.nw` | Electron/NW.js → §2.13 |
| Dados em texto puro, sem render 3D em tempo real | **engine de dados** → §2.14 |

## 0.3 Verificar a comunidade atual (5 min)

Versões andam. **Não instale de memória.**

- A wiki de modding do jogo, Nexus/Thunderstore/mod.io/Workshop, GitHub
- Qual loader a comunidade usa **para esta versão** (tModLoader, SMAPI, BepInEx, UE4SS,
  REFramework, SKSE, Fabric, ModEngine2/me3)
- Se já existe um loader com API de hooks → **use-o**, não faça do zero

## 0.4 ⚠️ GATE DE SEGURANÇA — passe isto antes de continuar

> **Este gate pode encerrar o projeto. Não o contorne.**

| Situação | Veredicto |
|---|---|
| O jogo tem componente **online/competitivo** | Só single-player, ou servidor que o usuário roda |
| **Anti-cheat** de kernel ou user-mode (EAC, BattlEye, Vanguard, EA Javelin, Ricochet, ACE, nProtect, XIGNCODE, mhyprot) | ⛔ **PARE.** Injeção de código é linha vermelha |
| Só existe uma rota **desativando/bypasseando** anti-cheat ou DRM | ⛔ **PARE.** Não faça bypass |
| Precisa de **spoof de hardware ID** ou mexer em **Denuvo/Steam stub** | ⛔ **PARE** |
| Denuvo presente mas **não bloqueia** a injeção | ✅ Pode seguir offline (foi o caso do Wukong) |
| Offline com anti-cheat, mas existe **opção oficial offline** sem ele | ✅ Use **só** a opção oficial |

Offline/multiplayer com anti-cheat é forbidden por regra, não por dificuldade. Se o usuário pedir
isso, recuse e ofereça single-player ou uma superfície de modding oficial.

⚠️ **Ferramentas ao lado de jogo protegido disparam o anti-cheat mesmo sem você tocar nele.** Feche
jogos protegidos antes de sessões de RE.

## 0.5 As quatro perguntas (A1–A4) — responda com evidência

Estas definem **se** e **como** é possível. Para cada uma: o que você achou, onde, e como confirmou.

| # | Pergunta | Como responder |
|---|---|---|
| **A1** | Consigo ler a **profundidade** do frame final? | RenderDoc num frame. Ache o depth attachment. Se só existe **um** attachment de cor e nenhum depth → o composite com oclusão está fora de alcance nesta rota |
| **A2** | Consigo **escrever a pose de câmera**? | Ache onde a view-projection é montada. Engine com câmera nativa → é o objeto de câmera. Unity → o `Transform` da `Camera`. Skyrim → `NiCamera` |
| **A3** | Consigo ler a **geometria de colisão**? | ⚠️ É o **recurso de colisão, não o de render.** Unity: `MeshCollider.sharedMesh`. Skyrim: `hkpShape` (Havok). Unreal: `UBodySetup`/`Chaos`. Source: as traces |
| **A4** | Consigo **empurrar dano/estado**? | Um método de dano que aceite override. Ache pelo que já muda a vida do jogador |

Registre em `MODLOG.md`. **Não pule A1** — ela é o gate da §0.6.

## 0.6 GATE DE PROFUNDIDADE — teste antes de planejar

> Este é o gate mais barato e o que mais mata projeto. Faça-o **antes** de escrever qualquer código
> de composição.

**Se sua escolha é γ (shader):** você **depende** da profundidade do host.

1. Instale ReShade com **suporte a add-on**
2. Coloque `DisplayDepth.fx` + `ReShade.fxh` na pasta de shaders
3. `DisplayDepth.fx` com `iUIPresentType == 2` desenha de propósito **normais na metade esquerda e
   profundidade linearizada na direita**
4. Marque **`Copy depth buffer before clear operations`** E **`Copy depth buffer during frame to
   prevent artifacts`**, reinicie
5. ⚠️ **Capture com o print do próprio ReShade, NÃO com captura de janela** (ver armadilha 23.1)
6. Meça quantas cores distintas tem cada metade

**Interpretação:**

```
metade esq (normais)   cores distintas = 1, tudo (127,127,255)   → NADA foi desenhado
metade dir (profund.)  cores distintas = 1, tudo (255,255,255)   → profundidade = 1.0 = VAZIA
```

⚠️ **Resultado medido no Black Myth: Wukong (UE5, D3D12):** ReShade 6.8 hooka perfeitamente,
`D3D12CreateDevice` e `CreateSwapChain` redirecionados, swapchain `DXGI_FORMAT_R10G10B10A2_UNORM`,
4 buffers — e mesmo assim a profundidade volta **completamente plana**. Reproduzido byte-idêntico
após reinício completo, com as duas opções ligadas. `ADDON_ADJUST_DEPTH` foi verificado e descartado.

| Resultado | Significado | O que fazer |
|---|---|---|
| **Gradiente com geometria** | γ viável | siga γ |
| **Plano (tudo 1.0)** | γ **inviável** nesta rota | vá para **δ** (§2 do apêndice B) — ou ache a profundidade com um add-on próprio (net-new, sem prior art) |

⚠️ Se sua engine é UE5/D3D12 e você precisa de γ: **considere δ como plano principal**, não plano B.

### 0.6b ⚠️ Proxy DLL — ONDE colocar (três falhas documentadas)

Este passo é falho o suficiente, e de formas diferentes, para vale Dedicated.

Um proxy `d3d9.dll`/`dxgi.dll`/`version.dll` **no lugar errado simplesmente nunca carrega** — e o
sintoma é "minha DLL faz nada, sem erro". ⚠️ Já há três casos documentados, **todos diferentes**:

| Jogo | Onde o proxy **não** funciona | Onde funciona | Por quê |
|---|---|---|---|
| **GTA V** | `dxgi.dll` na pasta do jogo | `ReShade64.asi`, via **ASI loader** | o jogo carrega o `dxgi.dll` **de sistema** antes de um proxy na sua pasta |
| **Portal 2** | `d3d9.dll` ao lado do `portal2.exe` | **`bin\d3d9.dll`** | o jogo carrega `shaderapidx9.dll` com **search path alterado** |
| **BepInEx** (Unity) | — | `winhttp.dll` (Doorstop) ou `version.dll` (MelonLoader) | o loader escolhe |

⚠️ **Procedimento antes de assumir que funciona:**

1. `dumpbin /imports <jogo>.exe` (ou PE-bear) — veja **de qual pasta e com qual nome** o jogo carrega
   o módulo que você quer sequestrar
2. Coloque o proxy **na subpasta que o jogo usa**, não na raiz, se o jogo carregar de subpasta
   (`bin`, `game/bin`, `<Mod>/`)
3. **Verifique que carregou** — um `OutputDebugString`/`Log` no `DllMain` ou uma chave de registro/
   arquivo marcador. Sem essa prova, "não fez nada" é indistinguível de "não carregou"
4. Se o proxy nativo não funcionar, use o **Ultimate ASI Loader** (ThirteenAG) — ele é um proxy
   pronto e carrega `*.asi`

⚠️ **Nunca confie em "funciona no meu PC".** Search path de DLL é influenciado por `PATH`, secure
boot, eKnownDLLs. Teste numa install limpa.

## 0.7 Montar o laboratório

```bash
um backup create "<pasta de saves>" --name <jogo>-saves    #paths vêm do um scan
```

- **Perfil separado do Minecraft, obrigatoriamente.** O mod sobrescreve opções (nuvens, bob, FOV
  effect, cap de fps) e cria/abre um mundo. Dê um `gameDir` próprio. Sem isso você destrói os mundos
  do usuário.
- **Windowed num tamanho de cliente conhecido** (registry/ini) para screenshots e coordenadas
  estáveis.
- **Código decompilado e assets extraídos fora do repo** (`~/<jogo>-decomp`), e `.gitignore` nos
  derivados. Nunca commite arquivos de jogo.
- **Anote o caminho de restauração** no `MODLOG.md`.

## 0.8 GATE 0 — só avance com tudo isto preenchido

- [ ] KB buscada
- [ ] Engine + loader + versão identificados
- [ ] **Gate de segurança passou** (§0.4)
- [ ] A1 respondida **com evidência** (captura ou dump)
- [ ] A2, A3, A4 respondidas
- [ ] **Gate de profundidade feito** (§0.6) — ou justificado como "não aplicável, chosei δ"
- [ ] Backup feito, laboratorio pronto
- [ ] `MODLOG.md` criado com paths, versões, decisões

**Saída: o RELÓTÓRIO DE RECON** ([template em §0.9](#09--template--relatório-de-recon))

## 0.9 TEMPLATE — Relatório de Recon

```markdown
# Recon: <Jogo> ↔ Minecraft

## Fixado
- Jogo: <nome> <versão/build>, Steam appid <id>
- Engine: <engine> <versão>
- Loader disponível: <qual> <versão>  /  inexistente
- Anti-cheat: <qual> — modo de teste: <como>
- Saves: <pasta>
- Log do host: <caminho>
- Log do Minecraft: <gameDir>/logs/latest.log

## A1 — Profundidade
<sim/não + evidência: capture, recurso, formato, quem tem o attachment final>
## A2 — Pose de câmera
<onde é montada; objeto/método; posso escrever? como?>
## A3 — Geometria de colisão
<recurso; formato; como extraio?>
## A4 — Dano/estado
<método; assinatura; posso interceptar e cancellar?>

## Gate de profundidade (se γ)
<resultado medido: cores distintas por metade, conclusão>

## Anti-cheat / legal
<veredicto; rota de lançamento offline>

## Decisão preliminar de rota
<α/β/γ/δ + eixo de câmera, com uma frase de justificativa>
```

---

# FASE 1 — ARQUITETURA

> Decida aqui. Não comece a codar a Camada 5 antes de fechar a Fase 1.

## 1.1 Eixo 1 — Quem é dono da câmera?

| | Descrição | Custo | Quem faz |
|---|---|---|---|
| **A · Minecraft manda** | O jogador está no Minecraft; a câmera do host é teleguiada | exige A2 (escrever pose) | ULTRAKILL, GTA |
| **B · Host manda** | O jogador está no host; o Minecraft é vitrine/servidor | **zero** hooks de escrita | SkyCraft |
| **C · Simétrico** | ambos respondem ao teclado, alternando foco | dois consumidores de input; frágil | raro |

⚠️ **Escolha B se A2 deu "difícil".** O SkyCraft é a prova de que B entrega um produto completo
(Skyrim com física, inventário e viewmodels do Minecraft), e custa menos porque você não precisa
escrever em nada do host — só ler.

## 1.2 Eixo 2 — Quem faz o desenho final?

**Esta é a pergunta que mais importa.** Quatro respostas:

| | Descrição | Precisa de profundidade do host? | Quem faz |
|---|---|---|---|
| **α · Overlay** | o frame vira textura num quad | **não** | primeiro marco, nunca entrega |
| **β · Depth-mesh** | o frame vira uma **mesh 3D real**; o Z-test do host resolve a oclusão | **sim** | ULTRAKILL |
| **γ · Shader** | shader injetado no pipeline testa a profundidade do convidado vs a do host | **sim** | GTA |
| **δ · Convidado desenha** | o host passa **geometria** para o convidado, que renderiza **dentro** do host | **não** | SkyCraft |

### Roteamento automático por engine

| Engine | A1 fácil? | **Modo recomendado** | Por quê |
|---|---|---|---|
| Unity Mono | sim (`MeshCollider`) | **β** | API de geometria completa, `Camera.onPreCull` |
| Unity IL2CPP | sim | **β** | idem, via Il2CppInterop |
| Skyrim / Creation | **não** (D3D11) | **δ** | foi o que o SkyCraft provou |
| Unreal | **arriscado** (veja Wukong) | **δ** se γ falhar; **β** via `SceneViewExtension` | UE5+D3D12 tem flat depth documentado |
| RE Engine | probe primeiro | **β/δ** | REFramework dá Lua com tipos gerenciados |
| FromSoftware | D3D12 | **δ** | me3 dá hooks de DLL; flat depth provável |
| RAGE / GTA | sim (reversed-Z) | **γ** | precedentado, funciona |
| Source 1/2 | trace-based | **β/δ** | colisão por trace, sem malha |
| .NET / XNA | varies | **β** se D3D9/DX11 exposto | |
| Godot | sim | **β** | |
| Nativo sem loader | quase nunca | **B + δ** ou **α** | ver §2.11 |
| JVM | N/A | **α ou β** | hooks de JVM são fáceis |

⚠️ **Regra de economia:** se A1 deu "não", só β e γ estão fora. **δ não precisa da profundidade** —
só das matrizes, para colocar geometria no lugar certo. Então δ é o plano, não o plano B.

## 1.3 Eixo 3 — Transporte

| | Quando |
|---|---|
| **JSON sobre WebSocket** | estado discreto. Sempre, para estado |
| **shm binário** | pixels, geometria, tudo que é grande |
| **LAN (e4mc / integrado)** | quando o convidado é multi-jogável (SkyCraft: até 100) |

⚠️ **Escolha JSON+shm (modos A/β/γ) a menos que o payload seja grande.** É mais fácil de debugar e
cobre 99% dos casos. Só vá para shm binário com rings nomeados se o orçamento de banda estourar
(~100 KB/frame de geometria é o ponto em que JSON morre — foi o que o SkyCraft fez).

## 1.4 Tabela de viabilidade e esforço

| Situação | Modo | Esforço realista |
|---|---|---|
| Unity Mono/IL2CPP com BepInEx | β | 4–8 semanas |
| **Qualquer Unity + ReShade, sem loader** | γ | ~2 semanas |
| Skyrim AE + SKSE (δ) | δ/B | ~1–2 semanas |
| GTA V Legacy + ScriptHookV | γ | ~2 semanas |
| Unreal com UE4SS / C# loader | β ou δ | 6–10 semanas |
| RE Engine / FromSoft | δ | 4–8 semanas |
| Source 1/2 | β/δ | 2–4 semanas |
| Godot | β | 2–3 semanas |
| Nativo sem loader | B + α/δ | 1 semana para B; A é irrealista |
| Outro JVM | α/β | ~1 semana |

⚠️ **"Semana" = meio tempo, para alguém que já fez modding antes.** Se for o primeiro projeto, dobre.

## 1.5 GATE 1 — só avance com o PLANO escrito

- [ ] Eixo de câmera escolhido, com justificativa
- [ ] Modo α/β/γ/δ escolhido, com justificativa (incl. o resultado do gate de profundidade se γ)
- [ ] Transporte escolhido
- [ ] Motor do-paced game: mods? nome, path, config
- [ ] O que você vai fazer **primeiro** (o vertical slice)
- [ ] Qual orACLE vai provar o passo 1
- [ ] `MODLOG.md` com a rota e a razão

**Saída: o PLANO** ([template em §1.6](#16--template--plano))

## 1.6 TEMPLATE — Plano

```markdown
# Plano: <Jogo> ↔ Minecraft

## Arquitetura
- Câmera: <A|B|C> — <razão>
- Render: <α|β|γ|δ> — <razão>
- Transporte: <JSON+shm | shm binário | shm+LAN>
- Escala: UNITS_PER_BLOCK = <?>  (medido: altura do host / altura do Steve)
- Ângulos: <tabela de conversão derivada>

## Camada 5 — o adaptador
- A1: <como>
- A2: <como>
- A3: <como>
- A4: <como>

## Vertical slice (primeiro)
<o menor objeto que prova o caminho inteiro>

## Oracles por passo
| Passo | Oráculo | Onde o resultado aparece |

## Fora de escopo (v1)
<lista explícita — evita scope creep>
```

---

# FASE 2 — CAMADA 5: O ADAPTADOR

> **Isto é o trabalho real.** As Camadas 1–4 são copiadas (Fases 3–8). Só isto é específico do jogo.

## 2.0 O contrato — escreva ANTES de implementar

```csharp
public interface IHostAdapter {
    // A1 — leitura de pixels
    bool TryCaptureFrame(out HostFrame f);          // cor + profundidade
    // A2 — escrita de câmera
    void WriteCamera(in HostPose pose);             // pos, rot, fov, near, far
    // A3 — leitura de geometria
    void PublishGeometry();                         // envia tris/trism/trisdel
    // A4 — empurrar estado
    void OnMinecraftDamage(double amount, Vector3 from, bool ranged);
    void OnMinecraftSwing(Vector3 eye, Vector3 dir);
    void OnMinecraftPlace(Vector3 at, Vector3 normal);
    // ciclo de vida
    void Start();  void Stop();                     // Stop() desfaz TUDO
}
```

Serve como **checklist** e como **documentação** do que você fez. Preencha os 4 slots acima com
respostas concretas antes de codar.

⚠️ `Stop()` precisa desfazer cada hook. Um plugin que deixa patch instalado corrompe o save do
usuário no próximo lançamento.

---

## 2.1 Unity Mono — BepInEx 5 / MelonLoader

**O caminho mais direto. É o ULTRAKILL inteiro.**

### Instalar o loader
```
1. Descompacte BepInEx 5 na pasta do jogo. Ele adiciona winhttp.dll (proxy Doorstop)
   e doorstop_config.ini.
2. Rode o jogo UMA vez para gerar BepInEx/config.
3. Plugins são assemblies .NET em BepInEx/plugins/.
4. Log: BepInEx/LogOutput.log  (ligue o console em BepInEx.cfg)
```

### Esqueleto do plugin
```csharp
using BepInEx;
using HarmonyLib;
using UnityEngine;

namespace MinecraftPassthrough;

[BepInPlugin("voce.seu.mod", "Minecraft Passthrough", "0.1.0")]
public class Plugin : BaseUnityPlugin {
    public static Plugin Instance; public static ManualLogSource Log;

    private class Runner : MonoBehaviour {          // ⚠️ não use Update no Plugin
        public Plugin p;
        void Update()  => p.Tick();
        void OnGUI()   => p.Gui();
        void OnApplicationQuit() { p.overlay.Disable(); p.link.Stop(); }
    }
    Runner runner;

    void Awake() {
        Instance = this; Log = Logger;
        new Harmony("voce.seu.mod").PatchAll();      // ⚠️ depois de bindar config
        Application.runInBackground = true;          // ⚠️ obrigatório
        link = new Link(Port);
        var go = new GameObject("PassthroughRunner") { hideFlags = (HideFlags)61 };
        UnityEngine.Object.DontDestroyOnLoad(go);
        runner = go.AddComponent<Runner>(); runner.p = this;
        Camera.onPreCull += OnPreCull;               // ⚠️ gancho de câmera
        Log.LogInfo("loaded. F10 toggles; play Steve in the Minecraft window.");
    }
}
```

⚠️ **Três regras que custam horas se você descobrir depois:**
1. **Não use `Load()`/`Unload()`** do BasePlugin — tudo em `Awake()`, com um `MonoBehaviour` aninhado
   para `Update`/`OnGUI`, `HideFlags = 61` + `DontDestroyOnLoad`.
2. `Application.runInBackground = true` — senão o jogo pausa quando o foco vai para o Minecraft.
3. `Camera.onPreCull` **mais** um prefix num sistema de câmera do host (§2.1.6).

### A2 — câmera

```csharp
// 1. o gancho global: roda antes de TODO render
Camera.onPreCull += cam => {
    if (cam == MonoSingleton<CameraController>.Instance.cam) ApplyCamera(cam);
};

void ApplyCamera(Camera cam) {
    cam.transform.SetPositionAndRotation(composite.CamPos, composite.CamRot);
    cam.fieldOfView = composite.Fov;
}

// 2. ⚠️ e DEPOIS de qualquer sistema que mova a câmera
[HarmonyPatch(typeof(PortalManagerV2), "LateUpdate")]     // nome do jogo-alvo
static class PortalCameraPatch {
    static void Prefix() => Instance.ApplyCamera(CameraController.Instance.cam);
}
```

⚠️ **Regra geral: aplique a pose no ponto mais tarde possível do pipeline do host.** `onPreCull` em
Unity, `Present` no DX11, o último passe antes do swapchain. Qualquer coisa antes, e algum sistema do
host te vence (portais, cutscenes, shake).

### A3 — geometria (só colliders, NUNCA renderers)

```csharp
static readonly Collider[] found = new Collider[8192];
const int MASK = /* LayerMaskDefaults.Get((LMD)1) | 1 | 0x40000 */;

int n = Physics.OverlapSphereNonAlloc(steve, Radius * Map.S, found, MASK,
                                       QueryTriggerInteraction.Ignore);

// whitelist
static bool Sendable(Collider c) =>
    c is MeshCollider || c is BoxCollider || c is SphereCollider || c is CapsuleCollider;

// por tipo:
//   MeshCollider  → mesh.vertices + mesh.triangles (36 B/tri), cached por mesh
//   BoxCollider   → 12 tri
//   Sphere/Capsule→ 112 tri
// ⚠️ mesh.isReadable == false → peça ILEGÍVEL: marque "duro", mande malha de fallback
```

⚠️ **Não Mande `mesh.colors32`, `normals`, `uv`.** Só posição. Cor no passthrough vem do framebuffer.
⚠️ **Exclua os blocos do próprio jogador** do stream (`c.transform.parent == blockRoot.transform`),
ou entra em loop de realimentação com 1 frame de atraso.

### A4 — dano

```csharp
[HarmonyPatch(typeof(NewMovement), "GetHurt")]          // nome do jogo-alvo
static class HurtPatch {
    static bool Prefix(int damage, bool instablack, bool explosion) {
        if (!Plugin.Active) return true;
        if (instablack || damage >= 1000) return true;   // ⚠️ nunca roube morte de cutscene
        if (Time.time < Plugin.Instance.graceUntil) return false;
        Plugin.Send(new { t = "hurt", d = damage * DamageScale });
        return false;                                    // cancela o nativo
    }
}
```

### Esconder o boneco do host

```csharp
// ⚠️ forceRenderingOff, não renomear nem destruir
foreach (var r in v1.GetComponentsInChildren<Renderer>(true)) r.forceRenderingOff = true;
// e um duplicado na layer de espelho para os mirrors mostrarem o Steve
```

⚠️ **Não use `DontDestroyOnLoad` sem re-aplicar no scene load.** Hook
`SceneManager.sceneLoaded` para reaplicar estado.

### Live inspection
- **UnityExplorer** (plugin BepInEx/MelonLoader): hierarquia, inspector e console C# in-game. O
  caminho mais rápido para achar o GameObject e o componente que você precisa.
- **BepInEx/MelonLoader log** + screenshot.

⚠️ **IL2CPP stripping:** métodos que o jogo nunca chamou podem não existir, e instanciações genéricas
podem faltar.

---

## 2.2 Unity IL2CPP — BepInEx 6

**Precisa do BepInEx 6 / Il2CppInterop, não do 5.**

1. BepInEx 6 gera assemblies de interop na primeira execução (lento, uma vez).
2. Patches via tipos `Il2Cpp*` gerados.
3. `MonoBehaviour`s gerenciados precisam de `ClassInjector.RegisterTypeInIl2Cpp<T>()`.
4. A API é a mesma (`MonoBehaviour`, `Mesh`, `Camera`); só o acesso muda.

**Leitura:**
```bash
# Cpp2IL ou Il2CppDumper com GameAssembly.dll + global-metadata.dat
# → nomes de tipos, métodos, campos, endereços, e DLLs dummy pro ILSpy
# corpos de método são NATIVOS: Ghidra/IDA com os scripts do Cpp2IL pra nomear funções
```

⚠️ Grupos de builds de loader são presos à versão do Unity. Use o que a comunidade recomenda para a
versão **deste jogo**.

---

## 2.3 Skyrim / Bethesda — SKSE

**O conjunto de hooks mais bem documentado que existe. O SkyCraft é a referência.**

### Instalar
- Plugins em `Data/SKSE/Plugins/*.dll`; log em `Documents/My Games/<Game>/SKSE/`
- **CommonLibSSE-NG** (Address Library) dá structs `RE::PlayerCharacter`, `RE::NiCamera`,
  `RE::TESObjectREFR` com offsets corretos para o build do usuário
- MO2 para isolamento (perfil por experimento), não instalação manual
- Fallout 76 é online-only: sem mods de cliente

### A2 — câmera e jogador (script nativo, xbyak)

```cpp
// O SkyCraft hooka o update do personagem e o da câmera
void PlayerUpdateHook::thunk(PlayerCharacter* pc, float delta) {
    // lê o estado do Minecraft do shm, reescreve pc->position in-situ
    SyncSneak(pc, sneaking, delta);
    PerFrame(pc, delta);
}
void PlayerCameraUpdateHook::thunk(PlayerCamera* cam) {
    ApplyMcFov(cam, mcFov);
    EnsureControlsEnabled();      // ⚠️ Skyrim desabilita input em menus/cutscenes
}
```

⚠️ **Sempre reabilite o input depois de aplicar a pose.** Sem isso o jogador fica preso sem
movimento em cada cutscene.

### A3 — colisão via Havok

```cpp
// ASSINATURAS: SkyCraft usa exatamente estas
void Collision::Collect(const RE::hkpShape* shape, const float*, const float[],
                        const float[], Job& job, int);
void Collision::Harvest(int cx, int cy, int cz);
void Collision::WorkerLoop();          // ⚠️ a colheita é cara: nunca na thread principal
```

⚠️ **Colisão em Skyrim é Havok (`hkpShape`), não a malha de render.** Se você usar a malha de render,
vai incluir portas, trigger volumes e FX.

### Render (SkyCraft δ)

- Hooks de render via `Present`/`Draw` do swapchain DX11
- `SetSkyrimShadows(FrameConstants&, const RE::NiPoint3&)`
- `DrawOverlay(IDXGISwapChain*)`, `EnsureTexture(w,h)`, `InitResources(IDXGISwapChain*)`
- Shaders próprios: `ShadowVS`, `ShadowPS`, `SkyShadowProbePS`
- `RE::NiCamera` dá as matrizes de view-projection

### ⚠️ Bugas específicas de Skyrim

1. **Atualizações do jogo quebram plugins.** Fixe a versão do jogo, ou use Address Library.
2. **Save bloat:** remover mods scriptados no meio de um save corrompe. Teste num save descartável.
3. **Content Club / "Anniversary"** muda os masters. Conheça sua lista de ESM.

---

## 2.4 Unreal — UE4SS / UE4SS-like

### Instalar
- **UE4SS**: `dwmapi.dll` (builds novos) ou `xinput1_3.dll` proxy + pasta `ue4ss/` ao lado do exe
- O que você ganha: mods **Lua** (`ue4ss/Mods/<Mod>/Scripts/main.lua`), **`RegisterHook`**
  (`"/Script/Engine.PlayerController:ClientRestart"`), `NotifyOnNewObject`, `FindFirstOf`,
  ler/escrever qualquer UProperty, chamar UFunctions, `ExecuteInGameThread`; mods **C++**; o
  **Live View** (dumper/inspector); gerador de SDK (Dumper-7 como alternativa)
- ⚠️ **Jogos UE customizados podem precisar de um override de config do UE4SS para os AOBs.**
- UE5 puramente IoStore precisa de **retoc**, não repak.

### Identificar
`um scan` faz grep do exe por `++UE5+Release-5.x`.

### A1 — o ⚠️ grande risco
Veja [§0.6](#06-gate-de-profundidade--teste-antes-de-planejar). **UE5 + D3D12 tem flat depth
documentado (Wukong).** Faça o teste antes de planejar γ.

Se γ falhar:
- **β** via um `SceneViewExtension` — você controla o passe, e pode ler a profundidade que a UE
  renderiza por um caminho que você controla
- **δ** via `USceneComponent`/proxy actors: crie atores nativos do host e deixe a UE iluminá-los

### A3 — colisão
`UBodySetup` / Chaos (`FBodySetup::AggGeom`) dá a malha de colisão sem tocar em assets. Alternativa:
raycast contra `UWorld::LineTraceSingleByChannel` em amostragem de grade — menos preciso, mas
funciona sem assets.

### A4 — dano
`UGameplayStatics::ApplyDamage` / `AActor::TakeDamage`. `UGameplayStatics::ApplyPointDamage` para
dano localizado.

### ⚠️ Bugas
1. **Paks cozidos na versão errada do UE crasham no mount.** Blueprint mods quebram a cada update.
2. **Arquivos `.sig` verificam paks.** Bypasses da comunidade existem só para jogos específicos
   single-player — **não** bypassar onde é anti-tamper de online.
3. **Muitos jogos UE online têm EAC/BattlEye** → sem injeção.

---

## 2.5 Capcom RE Engine — REFramework

- `REFramework` (praydog) como `dinput8.dll` + pasta `reframework/`
- **Lua** em `reframework/autorun/*.lua` com acesso ao sistema de tipos gerenciado:
  `sdk.find_type_definition`, `sdk.hook`, `sdk.call_native_func`
- Explorador de objetos in-game, câmera livre, VR em muitos títulos
- `REFramework-MCP` expõe o jogo a um agente

⚠️ **Online:** SF6 ranked, MH lobbies. Só cosmetics locais. Nunca mods que afetem gameplay online.

---

## 2.6 FromSoftware — ModEngine2 / me3

- **`me3`** (sucessor do ModEngine2, arquivado mas amplamente usado) carrega mods de uma pasta e
  **lança o jogo offline com EAC desligado**. ⚠️ Essa é a **única** forma aceitável. Nunca online.
- Mods de **código**: DLLs carregadas pelo mod engine, hooks via **MinHook**
- Dados/params: **Smithbox** (sucessor do DSMapStudio); **WitchyBND** para containers BND/DCX/BDT
- Seamless Co-op é rede separada

⚠️ Os clipes virais "Minecraft dentro do Elden Ring" e "creepers em Dark Souls" (setembro 2026) foram
feitos com Claude/Opus, e **o método não foi publicado**. A KB não tem nota. Você está na frente.

⚠️ **Elden Ring é D3D12** → leia [§0.6](#06-gate-de-profundidade--teste-antes-de-planejar) antes de
planejar γ. Provavelmente δ.

---

## 2.7 RAGE / GTA / RDR

**Precedentado: é o GTA V que tem o passthrough funcionando.** Copie de lá.

### Instalar
- **Story mode only.** BattlEye protege GTA Online; ScriptHookV recusa rodar online.
- **ScriptHookV** + **Ultimate ASI Loader** (`dinput8.dll`, plugins `*.asi`)
- **Natives DB** lista as funções de jogo callable
- Assets: **OpenIV** (nunca edite originais), CodeWalker para mapas

### ⚠️ Três coisas que só o GTA ensina

1. **ReShade tem que carregar pelo ASI loader.** O GTA carrega o `dxgi.dll` de sistema **antes** de um
   proxy na pasta dele, então o `ReShade64.dll` proxy nunca carrega. Instale como `ReShade64.asi`.
2. **Ciclo de vida do script:**
   - o menu de pausa **para** scripts ScriptHookV;
   - a câmera cinematográfica ociosa começa depois de ~30 s (chame `INVALIDATE_IDLE_CAM` todo frame);
   - o shake de explosão **não** é reportado por `IS_GAMEPLAY_CAM_SHAKING`, então
     `STOP_GAMEPLAY_CAM_SHAKING` não cancela.
3. **Timing de câmera:** o script lê a câmera do frame **sendo preparado**, 1 frame à frente do que
   está na tela. A correção é **reprojetar para a pose anterior** no compositor. Meça com uma cena de
   parede dourada (só-Minecraft) contra o horizonte (só-GTA).

### O compositor γ
`compositor.cpp` (ReShade add-on) sobe a textura do Minecraft para a GPU e seta uniforms de
`MCPassthrough.fx`. O shader faz: depth test contra o reversed-Z do GTA, **reprojeção** (rotação, e
depois 6-DoF com ray-march no depth), reiluminação a partir do blur da luz do GTA, grade de cor, haze
e blur de borda.

⚠️ **dev-c.com rejeita download scriptado** sem headers de browser. O `fetch_deps.sh` do exemplo
manda User-Agent/Accept.

⚠️ **dev-c.com e ReShade não são redistribuídos** — o exemplo só automatiza o download.

---

## 2.8 Source 1 / Source 2

### Identificar
Source 1: `gameinfo.txt`, `*_dir.vpk`, `bin/engine.dll`. Source 2: `engine2.dll`,
`gameinfo.gi`.

### Rotas
- **Source 1:** VScript (Squirrel) em `scripts/vscripts/*.nut`; Garry's Mod Lua;
  SourceMod+Metamod para servidores que você roda; **Source SDK 2013** constrói um mod de verdade
- **Source 2:** Workshop Tools (Hammer 2, ModelDoc, Particle Editor); addons em
  `game/<mod>_addons/`; `-tools` abre os editores; **VScript Lua** no Alyx

### A3 — colisão
⚠️ **Source não expõe malha de colisão de forma limpa.** Source 2 tem **VRF** (ValveResourceFormat)
que **decompila colisão de mapas**. Foi exatamente isso que os autores do `um` usaram para exportar a
colisão de mapas do CS2 para um clone de movimento. É a rota para β/δ em Source.

Alternativa mais barata: **trace-based**. Faça raycast do host para o Minecraft (`engine->TraceLine`)
e mande como barriers — foi o que o GTA fez (160 raycasts/frame, 40 blocos).

### ⚠️ VAC
Cliente modificado em servidor VAC = banido. Teste com `-insecure` num servidor local e **nunca**
injete em CS2/Dota/Deadlock/TF2 em matchmaking oficial.

### Oraclesosters
Demos (`.dem`) são ótimos: DemoFile.Net e bibliotecas de demoparser extraem estado por tick.

---

⚠️ **Antes de escrever hook nativo, teste o console de script.** Muitos jogos Source expõem uma
console que aceita comandos, e isso é **ordens de magnitude mais barato** que um hook:

- `hurtme N` (precisa de `sv_cheats 1`) — dano
- `script GetPlayer()...` — `SetVelocity` (voo, pulos de slime), `SetHealth` (cura, criativo)
- `r_drawviewmodel 0/1` — **esconder a arma enquanto você constrói**
- `prop_dynamic` com `solid 2`, `rendermode 10`, `SetSize` — caixa sólida invisível para Represents
  um bloco do convidado
- `logic_auto` com `OnLoadGame -> Kill` — mantém saves limpos

⚠️ **Isso é A2 e A4 resolvidos sem hook nenhum** em uma parte grande do projeto. E o console é
roteirizável, o que o torna oracle também.

⚠️ **Cuidado com `sv_cheats 1`:** o efeito colateral é que conquistas Steam não destravam naquela sessão. É
decisão do usuário.

⚠️ **Entidades não "soltam" no chão depois de um portal.** Ver armadilha 37: recreate, não teleport.

## 2.9 .NET / XNA — tModLoader / SMAPI

- **Terraria, Stardew Valley, Celeste, FTL** são .NET (XNA/FNA)
- **tModLoader:** app Steam gratuito (id 1281930). Conteúdo em C#: `ModItem`, `ModProjectile`,
  `ModNPC`, `ModSystem`, `ModPlayer`, `ModCommand`
- **SMAPI** para Stardew
- ⚠️ **tModLoader recusa a iniciar sem o app gratuito na biblioteca Steam do usuário. Adicione, não
  patcheie o check.**
- Leia com `ilspycmd -p -o ~/<jogo>-decomp <Game>.exe`

### Fator de sorte: FNA
Terraria roda em **FNA3D** (tradução de D3D9/XNA para D3D11). Isso significa:
- A API de render é **D3D11 real** → o caminho β funciona
- ⚠️ **FNA3D `ReadBackbuffer` vaza uma staging texture full-size por chamada** — 17 GB num take.
  **Nunca leia o back buffer de dentro do jogo.** Capture a janela de fora (ffmpeg
  `gfxcapture=hwnd=...`).

### A2 / A3
Reflita sobre `Main.player[0]`, `Main.camera`, e a API de tiles (`Tile`), ou
`Collision.CanCollision` / `SolidCollision` para colisão. Não há malha de triângulos — colisão é
grid de tiles. ⚠️ **Mapeie para barreiras ou shapes, não para triângulos.**

---

## 2.10 Godot

### Identificar
`.pck` ao lado do exe (magic `GDPC`) ou anexado ao próprio exe. `um scan` lê a versão no header.

### Rotas
1. **Godot Mod Loader** (`GodotModding/godot-mod-loader`) se o jogo tiver. Mods são zips em `mods/`
   com manifest; extensões hookam métodos via `extend`.
2. **PCK overlay:** `ProjectSettings.load_resource_pack("res://mod.pck")`. Precisa de execução de
   código para chamar.
3. **`override.cfg`** ao lado do exe:
   ```ini
   autoload/MyMod="*res://mod/my_mod.gd"
   application/run/main_scene="*res://mod/boot.tscn"
   ```
   ⚠️ funciona **só** se o script estiver dentro de um pack carregado.
4. **Rebuild** do projeto recuperado (GDRE Tools) — só uso pessoal, **nunca** redistribuir.

### A2 / A3
- Câmera: hook `Camera3D` / `_process` do nó
- Profundidade: `MeshInstance3D` com `depth_draw_mode = DEPTH_DRAW_ALWAYS`, ou render para um
  `SubViewport`
- Geometria: `PhysicsServer3D` / `CollisionShape3D`; `MeshDataTool` para malhas
- Input: `_input()`; `DisplayServer.window_set_mode(WINDOW_MODE_WINDOWED)`

### ⚠️ Bugas
1. **Versão do editor e do runtime precisam bater** (formato de recurso muda 4.2 → 4.3).
2. **GDScript 2.0 (Godot 4) ≠ 1.0 (Godot 3).** Linguagens diferentes.

---

## 2.11 Nativo sem loader — O CAMINHO DIFÍCIL

**Esta é a seção mais importante deste playbook para jogos AAA sem loader.** Leia com calma.

### Reality check

⚠️ **Para nativo, Modo B (o host manda a câmera) é quase sempre a única opção realista.** Escrever a
câmera de um engine custom sem loader é o problema original. Se o usuário quer Modo A, diga isso
antes de começar.

### 1. Conseguir código no processo

**Proxy DLL:** coloque uma DLL com o nome de uma que o jogo carrega da própria pasta:
`version.dll`, `dinput8.dll`, `winmm.dll`, `dxgi.dll`, `d3d9.dll`, `xinput1_3.dll`. Ela forwarda os
exports reais e roda seu código no `DllMain` (**spina uma thread; faça quase nada sob o loader lock**).

⚠️ **Confira os imports do jogo primeiro** com `dumpbin /imports` ou PE-bear. Colocar o proxy errado
não faz nada.

**Ultimate ASI Loader** (ThirteenAG) é um proxy pronto que carrega `*.asi` (DLL renomeada) da pasta
do jogo ou de `scripts/`. Muitos jogos antigos já usam.

Proton/Linux: `WINEDLLOVERRIDES="dinput8=n,b" %command%`.

### 2. Achar o que hookar

**Estático:** Ghidra ou IDA via MCP (skill `reverse-engineering`). Comece de **strings** (texto de UI,
mensagens de log, nomes de asset) e **imports** (D3D, XInput, APIs de arquivo). Siga cross-references
até a função que trata o que você quer mudar. **Nomeie funções e structs enquanto vai, e escreva no
`MODLOG.md`.**

> ⚠️ **Dica de RE que salvou o SkyCraft:** o build de release dele **preservou RTTI**. Só com extração
> de strings você recovers a arquitetura inteira — nomes de classe, de função, de campo, e os
> arquivos-fonte no caminho PDB. Antes de ir pro Ghidra, **tente extrair strings** e veja o que o
> binário já entrega de graça.

**Dinâmico:** Cheat Engine (value scan → "find what writes" → struct → owner), x64dbg, ReClass.NET,
Frida. Versões MCP de CE/x64dbg/Frida existem — **bind em 127.0.0.1**, não `0.0.0.0`.

**Assinaturas, não endereços:** ache funções por scan AOB/byte-pattern no startup, com wildcards para
relocação, para o mod sobreviver a updates. **Tenha fallback: log claro quando o padrão não é
encontrado.**

### 3. Hookear

MinHook, SafetyHook ou Microsoft Detours. Combine a calling convention exatamente (x64 é uma;
x86 precisa de `__thiscall`/`__stdcall`).

⚠️ **Mid-function hooks** (SafetyHook `MidHook`) mudam registradores numa única instrução — úteis para
capturar estado no ponto exato.

⚠️ **Hooks minúsculos.** Faça o trabalho pesado na sua própria thread ou out-of-process.

### 4. Desenhar e interagir

⚠️ **Este é o ponto de falha mais comum em nativo:** a DLL de sistema `dxgi.dll` costuma ganhar de um
proxy na pasta do jogo. Antes de assumir que o proxy funciona, **teste**. Se não carregar, use o
Ultimate ASI Loader (é o que o GTA exigiu).

- **Overlay/UI:** hook `IDXGISwapChain::Present` (D3D11/12) ou `vkQueuePresentKHR`, e desenhe Dear
  ImGui. Kiero acha esses rápido.
- **ReShade addon API** dá `present`, `draw_indexed`, render-target e **eventos de profundidade** sem
  você escrever os hooks.
- **Injetar 3D no passe do próprio jogo:** use as matrizes view-projection do jogo (ache em constant
  buffers com RenderDoc) e o depth buffer dele. RenderDoc (+ renderdoc-mcp) mostra qual passe desenha
  o quê.
- **Input:** hook o tratamento de input do jogo, ou raw input / XInput.


1. **ASLR:** compute endereços a partir da base do módulo em runtime. **Nunca** fixe endereços
   absolutos.
2. **Threads:** engines esperam chamadas na thread principal. Enfileire e rode de uma função
   per-frame hookeada.
3. **Crashes:** log em arquivo com flush. Instale um unhandled-exception filter que escreve minidump.
4. **Updates movem tudo.** Fixe a versão para desenvolvimento e **documente qual build você suporta**.

---

## 2.12 Outro jogo JVM

**O caminho mais fácil de todos.** As duas pontas são JVM.

- Decompile jars com **Vineflower** / CFR / Procyon, ou **Recaf** (edita + decompila)
- Loaders da comunidade: Slay the Spire (ModTheSpire + BaseMod, `@SpirePatch`), Starsector (API
  oficial), Project Zomboid (Lua + Java)
- Sem loader: **Java agent** (`-javaagent`) com ASM/ByteBuddy, ou **Mixin**

### Reuso direto
Você pode reusar a Camada 1–3 quase **literalmente**: `HostLink` é um `WebSocketServer` (troque o
pacote), `FrameExporter`/`SharedMemory` são FFM (idênticos), e o dispatcher é o mesmo. **Só a câmera e
a colisão mudam.**

---

## 2.13 HTML5 / Electron / NW.js / LÖVE

- **Electron:** `resources/app.asar`. Extraia com `npx @electron/asar extract app.asar app/`,
  patcheie o JS, e reempacote **ou** renomeie para a pasta `resources/app/` ser usada.
- **NW.js:** `package.nw` ou arquivos soltos. Devtools por
  `"chromium-args": "--remote-debugging-port=9222"` no `package.json`.
- **Construct / Phaser / PixiJS:** a lógica é JS puro.
- **Jogos de browser que você controla:** o console de devtools é seu loader de mod.
- **LÖVE (Lua):** o jogo é um zip (`.love` ou dados anexados). Descompacte e leia o Lua. Para mods sem
  repackar: **lovely** (injeção de patch Lua) + Steamodded (Balatro usa isso).
- **Cocos2d-x / Defold / HaxeFlixel:** ver `misc-engines.md`.

### A1/A2
⚠️ **Estas engines não têm "depth buffer do host" no sentido 3D.** Um jogo Electron é um navegador.
γ e β são difíceis; **δ é natural** (você pode injetar `<canvas>` e WebGL). Ou α (canvas por cima).

---

## 2.14 Engines de dados — RESPONDA NÃO

**Estas não são candidatas a passthrough.** Não têm render 3D em tempo real para compor.

| Engine | jogos | rota real |
|---|---|---|
| **Paradox** (Clausewitz/Jomini) | EU4, CK3, HOI4, Stellaris, Victoria 3 | mods de **script em texto puro** |
| **Genie** | Age of Empires II DE | mod de **dados** (`.dat` via genieutils) |
| **RPG Maker** | MV/MZ/XP/VX | plugins JS / RGSS |
| **Ren'Py** | visual novels | `.rpy` extras |
| **GameMaker** | Undertale, Deltarune | UTMT sobre `data.win` |

**O que dizer ao usuário:** "esse jogo não tem renderer 3D para compor, então passthrough não se aplica.
A alternativa é um mod de dados no próprio jogo — veja §2.14.1."

### 2.14.1 O que oferecer no lugar

Um mod de dados no próprio jogo: um script `.mod`/`.txt` na pasta de mods, ou uma edição de `.dat`.
GDScript em pck overlay, RGSS em `Data/Scripts.rxdata`, JS em `js/plugins/`, GML recompilado com UTMT.
Ferramentas: `genieutils-py` (Genie), GDRE Tools (Godot), UTMT (GameMaker), LibLCF (RPG Maker 2000), `unrpyc`/`unrpa` (Ren'Py).

Exceção: **GameMaker YYC e qualquer engine de dados com um runtime 3D embutido** caem em §2.11 — então passam a ser candidatos legítimos a passthrough.

### 2.15 Orquestrador de ciclo de vida — o padrão do launcher

> **Quem gerencia quem inicia e quem encerra é uma decisão de projeto, e ela é gratuita se você
> pensa nisso antes.**

O padrão que funciona:

```
lançador
  1. instala os arquivos de proxy/loader (e sabe removê-los)
  2. inicia o CONVIDADO oculto, espera o arquivo "status=running" dele
  3. inicia o HOST pela Steam / plataforma
  4. quando o HOST sair: pede ao convidado para salvar e encerrar, e REMOVE os arquivos de proxy
  5. watchdog: se um dos lados morrer, encerra o outro
```

Por que isso importa:

- **O proxy é temporário.** Removido no fim da sessão, nunca fica no install do usuário. Bom para
  segurança ([§0.4](#04--gate-de-segurança--passe-isto-antes-de-continuar)) **e** bom para coexistir
  com outros mods.
- **O convidado roda oculto** — `SDL_HideWindow`, show=false, ou só o shm sem janela. Ninguém vê
  duas janelas.
- ⚠️ **O watchdog correto é o processo, não o heartbeat de frame.** Documentado: *"Minecraft
  quitou enquanto o Portal 2 estava minimizado — o watchdog usava o heartbeat de frame."*
  Um jogo minimizado não produz frame. Verifique que o `.exe` existe.
- ⚠️ **Nome de usuário fixo.** *"Achievements e stats resetavam a cada lançamento — o launcher
  escolhe um nome `PlayerNNN` aleatório, então um UUID novo cada vez."*

⚠️ **Efeito colateral que você vai escolher sem querer:** dar `sv_cheats 1` num jogo de Steam
**desabilita as conquistas Steam naquela sessão**. Isso é do usuário decidir — diga, não decida.

---


---

# FASES 3–8 — CAMADAS REUTILIZÁVEIS

> **Copie, não reescreva.** Isto é spec extraída de binários de três implementações que funcionam.
> É ~85% do trabalho e é genérico para qualquer jogo.

| Camada | Onde | Índice |
|---|---|---|
| 3 · Core | `Link`, `Map`, seqlocks | [§3](#fase-3--camada-1--core) |
| 4 · Protocolo | `HostLink`, tabelas | [§4](#fase-4--camada-2--protocolo) |
| 5 · Transporte | `FrameExporter`, `SharedMemory`, ABI | [§5](#fase-5--camada-3--transporte-de-pixels) |
| 6 · Janela/input | `WindowOverlay`, `InputBridge` | [§6](#fase-6--camada-4--janela-e-input) |
| 7 · Colisão | `UkGeometry`, `TriShape`, `Voxelizer` | [§7](#fase-7--colisão) |
| 8 · Combate | `Combat`, `Enemies`, `MobWar` | [§8](#fase-8--combate-bidirecional) |

---

### 2.16 Instalador e assets gerados no PC do usuário

A forma mais limpa de resolver "não posso distribuir assets" é **não distribuir nada e gerar no
install do usuário**. O que o Portalcraft faz, e é o padrão a copiar:

**O instalador:**
- é um zip pequeno (~0,4 MB): scripts + o jar do mod + o add-on + o shader
- baixa **Temurin**, **Minecraft** (version JSON da Mojang, bibliotecas e assets com `sha1`
  verificado), **Fabric** (meta profile) e **ReShade** (`sha256` pinado)
- ⚠️ **nunca executa o instalador do ReShade**: extrai o payload do fim do registro de diretório
  central dentro de `ReShade_Setup_*.exe`

**Os assets são gerados na máquina do usuário, do **próprio jar do Minecraft dele**:**
logo (fonte do Minecraft + concreto branco + pedra + um portal desenhado), ícone (bloco de grama
isométrico), painel do menu principal, e quatro telas de loading (parede de sala de teste com
blocos de Minecraft e um portal azul e um laranja). System.Drawing.

⚠️ Isso é melhor do que "converter do install do usuário" porque é **determinístico e pequeno** —
não depende de um formato de asset reversado.

⚠️ **Lição de instalador (custou um bug de release):** *"o profile meta do Fabric dá `sha1` e
`size` para cada biblioteca **exceto** `net.fabricmc:fabric-loader`; o instalador leu 'sem hash'
como 'nada a buscar'."* Corrija: **quando uma entrada não tem hash, baixe `<artifact url>.sha1` do
maven e verifique contra isso.** E ⚠️ **teste lançar a cópia instalada, não só instalar.**

# FASE 3 — CAMADA 1: CORE

## 3.1 Quem é servidor

**Minecraft é o servidor WebSocket** (nos modos A/β/γ). Não é arbitrário: o Minecraft já roda um
servidor integrado e um loop de tick confiável, e você não quer que uma falha do host derrube o mundo.

```java
private HostLink(int port) {
    super(new InetSocketAddress("127.0.0.1", port));
    this.setReuseAddr(true);
    this.setDaemon(true);
}
static void launch() {
    int port = Integer.getInteger("passthrough.port", 25599);
    instance = new HostLink(port);
    instance.start();
    Passthrough.events = message -> instance.broadcast(message);  // 1 writer, N readers
}
```

O host é o cliente e reconecta para sempre:

```csharp
private void Run() {
    while (!stopping) {
        var ws = new ClientWebSocket();
        try {
            ws.ConnectAsync(uri, CancellationToken.None).Wait(3000);
            if (ws.State != WebSocketState.Open) { Thread.Sleep(1000); continue; }
            connected = true;
            var rx = Task.Run(() => Receive(ws));
            while (!stopping && ws.State == WebSocketState.Open && !rx.IsCompleted) {
                wake.WaitOne(50);
                while (outbox.TryDequeue(out var msg))
                    ws.SendAsync(Bytes(msg), WebSocketMessageType.Text, true, None).Wait();
            }
        } catch (Exception ex) { if (connected) Log(ex); }
        finally { connected = false; ws.Dispose(); }
        Thread.Sleep(1000);
    }
}
```

⚠️ **`Send()` nunca bloqueia:**

```csharp
public void Send(string json) {
    // DESCARTA quando o Minecraft está lento ou ausente.
    if (connected && outbox.Count <= 2000) { outbox.Enqueue(json); wake.Set(); }
}
```

Um backlog ilimitado vira OOM com o jogo-alvo fechado. Descarte é o comportamento correto para
mensagem de estado.

## 3.2 Heartbeats e detecção de instância morta

```java
// Header do shm, 64 bytes
H_MAGIC = 0, H_VERSION = 4, H_SKYRIM_PID = 8, H_MC_PID = 12,
H_SKYRIM_HEARTBEAT = 16, H_MC_HEARTBEAT = 24;   // GetTickCount64 em ambos

static boolean active() {
    long beat = LONG.getAcquire(shm, 16L);
    return tickCount() - beat < 8000L;              // 8 s
}
LONG.setRelease(shm, 24L, tickCount());           // a cada frame
```

⚠️ **Detecção de instância trocada** — a peça que ninguém tem. Se o PID do host muda, foi restart
dele, e todo o estado (geometria, atlas, atores) é inválido:

```java
if (SkyLink.skyrimPid() != lastSkyrimPid) {
    generation++;
    WorldExporter.resendEverything();
    Log.info("SkyCraft: Skyrim instance changed (pid {})", SkyLink.skyrimPid());
}
```

Sem isso você fica com geometria do level anterior, visivelmente errada e sem mensagem de erro.

## 3.3 Mapeamento afim de coordenadas

```csharp
public static class Map {
    public static float S = 2f;              // unidades do host por bloco
    public static double BaseX = 4096.0;     // faixa X do level
    public static double BaseY = 100.0;      // Y do chão
    public static int Version;               // token de invalidação

    // X é ESPELHADO — daí o -1/S na matriz.
    public static Vector3 ToUnity(double x, double y, double z)
        => new((float)((BaseX - x) * S), (float)((y - BaseY) * S), (float)(z * S));

    public static Quaternion Rot(double yaw, double pitch)
        => Frame * Quaternion.Euler((float)pitch, (float)yaw, 0f);

    public static void McAffine(double[] P, double[] t) {
        var f = Frame * Quaternion.Inverse(new Quaternion(0,0,1,0));
        P[0] = -f.m00 / S;  P[1] = f.m01 / S;  P[2] = f.m02 / S;
        P[3] = -f.m10 / S;  P[4] = f.m11 / S;  P[5] = f.m12 / S;
        P[6] = -f.m20 / S;  P[7] = f.m21 / S;  P[8] = f.m22 / S;
        t[0] = BaseX; t[1] = BaseY; t[2] = 0.0;
    }
}
```

⚠️ **Mede `UNITS_PER_BLOCK`, não chute.** Os três projetos usam **2**, **1** e **70**:

| | mapeamento | `UNITS_PER_BLOCK` |
|---|---|---|
| ULTRAKILL | V1 (3.5 u) ≈ 1.8 blocos | **2** |
| GTA | 1 metro = 1 bloco; GTA (x,y,z) → MC (x, z+off, −y) | **1** |
| SkyCraft | 1 bloco = 70 unidades SKSE | **70** |

```csharp
float v1Height = nm.GetComponent<CapsuleCollider>() is CapsuleCollider cc
    ? (cc.height * 0.5f - cc.center.y) * nm.transform.lossyScale.y : 1.75f;
float steveScale = (v1Height / hostUnitsPerBlock) * 0.97f;
```

Aplique nos **dois** lados — um float que existe num só lugar sempre diverge:

```csharp
Command($"execute as @a run attribute @s minecraft:scale base set {steveScale:F3}");  // MC
mirrorDouble.Scale = steveScale;                                                   // espelhos
```

**BaseX por level** (para dois levels não se sobreponham):

```csharp
uint h = 2166136261u;                          // FNV-1a
foreach (char c in scene) h = (h ^ c) * 16777619u;
BaseX = (h % 2000 + 1) * 4096.0;
BaseY = 100.0 - Feet(Singleton).y / S;
Version++;
```

## 3.4 Hash de chave de célula (idêntico nos dois lados)

```csharp
public static long Key(int x, int y, int z)
    => ((long)(x & 0x3FFFFFF) << 38) | ((long)(y & 0xFFF) << 26) | ((long)(z & 0x3FFFFFF);
```
```java
static long key(int x, int y, int z)   // UkGeometry — 22/20/22 bits, grid de 16 blocos
    => ((long)(x & 4194303) << 42) | ((long)(y & 1048575) << 22) | (z & 4194303);
```

⚠️ Divergir aqui produz "o Steve atravessa o chão em determinados cantos" e nada mais — sem erro, sem log.

## 3.5 Seqlocks

Todas as três implementações usam. É o mesmo padrão.

```java
// ESCRITOR
INT.setRelease(shm, seqOff, seq + 1);      // ímpar = escrevendo
VarHandle.storeStoreFence();
/* ... campos ... */
VarHandle.loadLoadFence();
INT.setRelease(shm, seqOff, seq + 2);      // par = completo

// LEITOR
for (int i = 0; i < 1000; i++) {
    int s1 = INT.getAcquire(shm, seqOff);
    if ((s1 & 1) != 0 || s1 == 0) { Thread.onSpinWait(); if (i > 100) Thread.yield(); continue; }
    leiaOsCampos();
    VarHandle.loadLoadFence();
    if (INT.getAcquire(shm, seqOff) == s1) return campos;
}
return null;
```

SkyCraft usa `1000` retries para `SKY_STATE`, `100` para `WATER_GRID`, `16` para `ACTOR_TABLE`. O
número é **quanto você quer esperar** antes de render um frame sem aquele dado.

## 3.6 Anel SPSC de bytes (payload grande)

```java
long head = LONG.getAcquire(shm, HEAD_OFF);
long tail = LONG.getAcquire(shm, TAIL_OFF);
long pos  = head % RING_BYTES;
long msg  = (8 + payload + 7) & -8L;                    // alinha a 8
if (pos + msg > RING_BYTES) { escrevaPad(pos); pos = 0; }
if (RING_BYTES - (head - tail) < msg + 8) return false; // cheio
/* ... type, payload, header, body ... */
LONG.setRelease(shm, HEAD_OFF, head + msg);
```

⚠️ **Bug real do SkyCraft:** o wrap usa `head % 67_108_736` mas a checagem de espaço usa
`67_108_864` — 128 bytes de divergência. Use **a mesma constante** nas duas.

---

# FASE 4 — CAMADA 2: PROTOCOLO

## 4.1 Formato

JSON com um único discriminador `"t"`. **Todo array vira array plano de números** — nunca array de
objetos.

```java
// ERRADO: 40 bytes de chave por inimigo, alocação por inimigo
{"t":"enemies","e":[{"id":1,"x":0.5,"y":2.0,"z":-3.0,"w":0.6,"h":1.8}, ...]}
// CERTO: 6 doubles por inimigo, zero alocação
{"t":"enemies","e":[[1,0.5,2.0,-3.0,0.6,1.8], ...]}
```

## 4.2 Dispatcher

```java
public void onMessage(WebSocket conn, String message) {
    JsonObject m = JsonParser.parseString(message).getAsJsonObject();
    HostState.heard();                                // mantém o link "anexado"
    switch (m.get("t").getAsString()) {
        case "cam":     HostState.update(m); break;
        case "tris":    UkGeometry.put(m.get("id").getAsInt(), m.get("d").getAsString(),
                                       doubles(m.getAsJsonArray("m"))); break;
        case "shaped":  WorldBridge.shaped(ints(m.getAsJsonArray("c"))); break;
        case "hurt":    WorldBridge.hurtPlayer(...); break;
        case "gta": case "gtastate": case "gtainfo": case "director":
            relay(conn, message); break;              // fan-out
        default:
            Minecraft mc = Minecraft.getInstance();
            mc.execute(() -> ClientInput.handle(mc, m));   // qualquer mensagem nova vira callback
    }
}
```

**Três decisões embutidas:**
1. ⚠️ **`case "cmd"` não tem `break`** no ULTRAKILL — cai em `hb`. Bug inofensivo, publicado. Não copie.
2. **`default:` → input do cliente** mantém a Camada 4 estável enquanto você inventa mensagens.
3. **Toda mensagem recebida chama `heard()`.** Sem isso você precisa de heartbeat dedicado nas duas
   direções.

## 4.3 Tabela de mensagens

**Crie a sua desde o dia 1**, mesmo que 80% comece vazio.

**Host → Minecraft:**

| `t` | Payload | Handler |
|---|---|---|
| `cam` | pose completa da câmera | `HostState.update` |
| `tris` | `id`, `d` (b64 LE f32), `m[12]` | geometria estática |
| `trism` | `b` (b64 LE doubles, passo 13) | peça **movida** |
| `trisdel` | `ids[]` | remoção |
| `trisclear` / `holes` | — / `b[]` (6 por buraco) | clear / portais |
| `shaped` | `c[]` (4 ints: x,y,z,código) | grade de blocos |
| `unsolid` | `c[]` (3 ints) | clear de células |
| `clear` / `cmd` / `hb` | — / `c` / — | |
| `hold` | `on` bool | congela o Steve |
| `enemies` | `e[][]` (6 doubles) | avatares de inimigo |
| `hurt` / `heal` | `d`, `pos[3]`, `proj` | dano |
| `blocksync` | `r` raio | re-sync |
| `projhit` | `id`, `pos[3]`, `stick` | projétil |
| `spawnmobs` | `k`,`n`,`rmin`,`rmax`,`arc`,`yaw`,`at[3]` | ondas |

**Minecraft → Host:**

| `t` | Payload | Efeito |
|---|---|---|
| `steve` | `pos, ground, yaw, hp, max, food, gm, dead, held, gs, vel` | pose + HUD |
| `swing` | `eye[3], look[3], item, creative` | atacar / clicar |
| `uhit` | `id, d, from[3], kind, item, air, sprint, riptide, glide, mob` | melee → dano |
| `proj` | `[id, kind, x, y, z, dmg, mob]` | projétil |
| `explosion` | `pos[3], r, src` | explosão real |
| `skin` | `name, slim, w, h, rgba(b64)` | pele p/ espelhos |
| `blocks` | `set[3n], clear[3n]` | suas construções |
| `died` / `totem` / `blocked` / `inportal` / `pteleport` | | |

**Camada 5** (você inventa): `place`, `warp`, `launch`, `key`, `slot`, `scroll`, `hud`, `view`, `look`.

## 4.4 Modo δ: binário com regiões nomeadas

Quando o payload é grande, JSON não serve. Declare um **layout de regiões**:

```java
public static final int    MAGIC = 1129925459;
public static final String MAPPING_NAME = System.getProperty("skycraft.link", "Local\\SkyCraft_v1");
public static final double UNITS_PER_BLOCK = 70.0;
public static final long OFF_HEADER = 0, OFF_SKY_STATE = 256, OFF_MC_STATE = 512;
public static final long OFF_WATER_GRID = 1024, OFF_OVERLAY_CTL = 768, OFF_OVERLAY_SLOT_HDR = 832;
public static final long OFF_INPUT_RING = 4096, OFF_ACTOR_TABLE = 73728, OFF_EVENT_RING = 94208;
public static final long OFF_WORLD_ENTITIES = 114688, OFF_COLLISION_RING = 131072;
public static final long OFF_OVERLAY_PIXELS = 33685504, OFF_RENDER_RING = 133218304;
public static final long MAPPING_BYTES = 200327168L;
```

**A aritmética fecha — verifique no papel antes de codar:**

```
131 072 + 33 554 432           = 33 685 504
33 685 504 + 3 × 33 177 600   = 133 218 304
133 218 304 + 67 108 864       = 200 327 168
33 177 600 = 3840 × 2160 × 4
```

**Vantagem de depuração:** os offsets têm nome — `SS_POS_X`, `MS_EYE_HEIGHT_T`, `WE_SEL_MIN`. Você
consegue explicar cada byte numa frase. JSON mal documentado, não.

---

# FASE 5 — CAMADA 3: TRANSPORTE DE PIXELS

## 5.1 Por que shm, não JPEG

Um frame 4K RGBA são ~33 MB. Base64 no WebSocket a 300 ms de latência não dá 60 fps.
`copyTextureToBuffer` + `MemorySegment.copy` escreve **direto no mapeamento**, sem array no heap.

Se precisar de compressão, use **BCn/ASTC/ETC2 por hardware**. Não use JPEG/H.264 no caminho crítico.

## 5.2 A ABI

```
Nome:      Local\MCPassthroughFrame
Tamanho:   298 602 496 bytes (284 MB) = 4096 + 3 slots × 3 planos × 33 177 600
Magic:     1414546253 = 0x5450434D = "MCPT"
```

**Header global:**

| Offset | Tipo | Valor | Significado |
|---|---|---|---|
| 0 | int32 | `0x5450434D` | magic |
| 4 | int32 | 1 | versão |
| 8 | int32 | 4096 | header |
| 12 | int32 | 3 | slots |
| 16 | int64 | 99 532 800 | stride |
| 24 | int32 | 3840 | largura máx |
| 28 | int32 | 2160 | altura máx |
| 32 | int64 | — | contador de publicação |
| 40 | int32 | −1 | slot publicado |
| 44 | int32 | — | **PID do Minecraft** (o host acha a HWND por ele) |

**Descritor de slot** — `256 + 128 × slot`:

| Offset | Tipo | Campo |
|---|---|---|
| +0 | int64 | `seq` — **ímpar = escrevendo, par = válido** |
| +8/+16 | int64 | frame no produtor / no host |
| +24/+28 | int32 | `W`, `H` |
| +32/+36 | float32 | near (0.05), far (ao vivo) |
| +40 | float32 | **FOV vertical em graus** |
| +44 | int32 | flags |
| +48/+56/+64 | float64 ×3 | posição da câmera no mundo do MC |
| +72/+76/+80 | float32 ×3 | yaw, pitch, roll |
| +84 | int32 | primeira pessoa? |
| +88/+96 | int64 | capture / publish nanos |
| +104/+112/+120 | float64 ×3 | **pés** do jogador (pode ser NaN) |

**Payload** — `4096 + 99 532 800 × slot`:

```
+0             cor         RGBA8, W*H*4
+W*H*4         profundidade float32
+2*W*H*4       overlay     RGBA8   (HUD, chat, mão, UI)
```

**Flags:** bit0 = profundidade em `[0,1]`; bit1 = reservado (escreva `2`); bit2 = reversed-Z.

**Por que 3 planos:** o produtor **limpa a cor para transparente** depois de copiar, então o HUD/chat/
mão vão num backdrop transparente — e esse é o segundo plano.

## 5.3 O produtor (Java FFM)

```java
private static final Linker LINKER = Linker.nativeLinker();
private static final SymbolLookup K32 = SymbolLookup.libraryLookup("kernel32", Arena.global());
private static final MethodHandle CREATE = LINKER.downcallHandle(
    K32.find("CreateFileMappingW"), FunctionDescriptor.of(ADDRESS, ADDRESS, ADDRESS,
        JAVA_INT, JAVA_INT, JAVA_INT, ADDRESS));
private static final MethodHandle MAP = LINKER.downcallHandle(
    K32.find("MapViewOfFile"), FunctionDescriptor.of(ADDRESS, ADDRESS,
        JAVA_INT, JAVA_INT, JAVA_INT, JAVA_LONG));

static SharedMemory open(String name, long size) {
    var arena = Arena.global();
    byte[] utf16 = (name + "\0").getBytes(StandardCharsets.UTF_16LE);
    var wide = MemorySegment.ofArray(utf16).reinterpret(utf16.length);
    var handle = (MemorySegment) CREATE.invoke(
        MemorySegment.ofAddress(-1L), MemorySegment.NULL, PAGE_READWRITE,
        (int)(size >>> 32), (int)size, wide);
    var view = (MemorySegment) MAP.invoke(handle, FILE_MAP_ALL_ACCESS, 0, 0, size);
    return new SharedMemory(view.reinterpret(size));
}
```

Requires `--enable-native-access=ALL-UNNAMED`.

⚠️ **Só 7 syscalls no total** (SkyCraft): `OpenFileMappingW`, `MapViewOfFile`, `GetTickCount64`,
`GetCurrentProcessId`, `QueryPerformanceCounter`, `QueryPerformanceFrequency`, `CreateMutexW`.

⚠️ **`nanoTime`, nunca `GetTickCount`.** `GetTickCount` tem passos de ~16 ms e **judder** no frame
pacing. `nanoTime` == `QPC` no Windows.

## 5.4 O ring (anti-tearing)

```java
static void publish(Capture c) {
    int slot = slotNext; slotNext = (slotNext + 1) % 3;
    MemorySegment m = shm.segment;
    long desc = 256L + 128L * slot;

    long seq = m.get(LONG, desc);
    if ((seq & 1L) != 0L) seq++;        // nunca sobrescreve um odd em voo
    m.set(LONG, desc, seq + 1L);        // 1. ÍMPAR = escrevendo
    VarHandle.fullFence();              // 2. release
    long base = 4096L + 99_532_800L * slot;
    int n = c.width * c.height * 4;
    copy(c.color, m, base, n);          // GPU → shm, DIRETO
    copy(c.depth, m, base + n, n);
    copy(c.overlay, m, base + 2 * n, n);
    /* ... descritor ... */
    VarHandle.fullFence();              // 3. release
    m.set(LONG, desc, seq + 2L);        // 4. PAR = completo
    m.set(INT, 40L, slot);
    VarHandle.fullFence();
    m.set(LONG, 32L, ++publishCounter);
}
```

```csharp
public bool Next(out Frame f) {
    long counter = *(long*)(b + 32);
    if (counter == lastPublish) return false;
    int slot = *(int*)(b + 40);
    if (slot < 0 || slot >= *(int*)(b + 12)) return false;
    long seq = *(long*)(b + 256 + 128 * slot);
    if ((seq & 1) != 0) return false;
    Thread.MemoryBarrier();                           // acquire
    /* ... parse ... */
    if (f.W <= 0 || f.H <= 0) return false;
    lastPublish = counter;
    return true;
}
public bool StillValid(in Frame f) {
    Thread.MemoryBarrier();
    return *(long*)(b + 256 + 128 * f.Slot) == f.Seq;
}
```

## 5.5 Três robustez obrigatórias

**a) Watchdog de slot preso** (fence da GPU perdido = pipeline travado para sempre):
```java
private static final long STUCK_NANOS = 1_000_000_000L;
if (!c.busy || now - c.busySince >= STUCK_NANOS) {
    c.generation++; c.busy = true; c.busySince = now;   // generation invalida callbacks atrasados
}
/* e no callback: */ if (c.generation != generation || !c.busy) return;
```

**b) Teto de tamanho:**
```java
if ((long) w * h * 4L > 33_177_600L) { warnOnce(); return; }
```

**c) Sem fallback de CPU.** Se `CreateFileMappingW` falhar uma vez, o export desliga para sempre:
```java
} catch (Throwable t) { failed = true; LOG.error("frame export disabled", t); return false; }
```

Um `glReadPixels` síncrono derruba o framerate para 5 fps. **Melhor não integrar nada do que integrar
devagar.**

## 5.6 Adaptando para outro produtor

**Vulkan:** `vkCmdCopyImageToBuffer` com `VK_IMAGE_ASPECT_DEPTH_BIT` → `R32_SFLOAT`; ou
`VK_EXT_copy_memory_indirect` se a imagem tiver `READ_ONLY_LATEST_ACCESS`.

**DX12:**
```cpp
D3D12_GPU_COPY_DESC dst = {};
dst.pResource = dstBuffer; dst.Type = D3D12_RESOURCE_TYPE_BUFFER;
dst.SubresourceLayout.Footprint = { DXGI_FORMAT_R32_FLOAT, w, h, 1, w*4 };
copyQueue->CopyTextureRegion(&dst, 0, 0, 0, srcDepth, &srcBox);
```

**OpenGL:** `glReadPixels` do depth attachment não é portátil — **renderize a profundidade para um FBO
de cor em `R32F`** num passo separado.

⚠️ **Se capturar cor DEPOIS de profundidade no backend GL do Minecraft, a cor volta lixo.** O
`copyTextureToBuffer` de profundidade deixa o read buffer em `GL_NONE` e nunca restaura. Correção de
3 linhas:

```java
// GlCommandEncoderMixin — só para fonte com aspecto de profundidade
@Inject(method = "copyTextureToBuffer(...)",
        at = @At(value = "INVOKE", target = "...GlStateManager;_glFramebufferTexture2D(IIIII)V"))
private void restoreReadBuffer(GpuTexture src, GpuBuffer dst, long off, Runnable cb,
        int mip, int x, int y, int w, int h, CallbackInfo ci) {
    if (src.getFormat().hasDepthAspect()) GlStateManager._glReadBuffer(36064);  // GL_COLOR_ATTACHMENT0
}
```

Foi o primeiro gotcha do passthrough GTA e reaparece em qualquer projeto.

## 5.7 Modo δ — Minecraft não desenha nada

A Camada 3 em δ transporta: **o atlas de texturas** (uma vez + `REN_ATLAS_REGION` para frames
animados), **geometria de 32 B/vértice**, e **um plano de UI** (o HUD do MC precisa aparecer).

```java
@Mixin(LevelRenderer.class)
class LevelRendererMixin {
    @Inject(method = "render(...)", at = @At("HEAD"), cancellable = true)
    void skipWorld(..., CallbackInfo ci) { if (SkyClient.linked()) ci.cancel(); }
}
```

⚠️ **Isso responde "e se o Minecraft rodar a 10 fps?"** Em δ, **não importa.** O Minecraft é servidor
de assets e física; quem desenha é o host, a 60 fps.

**O truque de textura que faz δ funcionar** — roube o atlas em memória:

```java
@Accessor("texturesByName") Map<Identifier, TextureAtlasSprite> skycraft$texturesByName();
@Accessor("originalImage") NativeImage skycraft$originalImage();   // ← PRÉ-atlas
```

`originalImage` dá a imagem **antes** de ser costurada no atlas: não reparseia PNG, e a tira de frames
animados fica intacta. Junte blocos+itens numa folha, **edge-replicate** para o sampler não puxar a cor
do vizinho no mip chain, e mande só UV:

```java
int pad = Math.max(0, Math.min(imageX - sprite.getX(), imageY - sprite.getY()));
for (int y = -pad; y < h + pad; y++)
    for (int x = -pad; x < w + pad; x++) {
        int argb = image.getPixel(imageX + Math.clamp((long)x, 0, w-1),
                                  imageY + Math.clamp((long)y, 0, h-1));   // edge replicate
        pixels.put((byte)(argb>>16)); (byte)(argb>>8)); (byte)argb; (byte)(argb>>>24);
    }
```

⚠️ **Envie os nibbles de luz (block/sky) por vértice.** Sem eles o shader do host não reproduz o termo
de iluminação suave do MC.

⚠️ **Assine o sombreamento do MC no vértice** (`unshade(argb, shade)`) e o shader do host só precisa
somar a luz própria sobre uma base já sombreada — nenhum shader do Minecraft precisa ser portado.

**Formato de vértice de 32 bytes:**

```
 0  f32 x,y,z        posição
12  f32 u,v          UV no atlas mesclado
20  u8  R,G,B,A      cor (sombreamento MC já aplicado)
24  u32 light        block | (sky << 8)
28  u32 flags        bit0 opaco, bit1 translúcido, bits4-7 = Direction.ordinal()+1
```

---

## 5.8 Reprojeção e predição de câmera

> **Esta é a seção mais ausente do playbook e o maior salto de qualidade entre "jogável" e "liso".**
> O passthrough GTA resolveu o lag de 1 frame *aceitando* um frame de atraso. O Portalcraft
> *prevê* e corrige. Medido: com as configurações do usuário, **70–80 % dos frames apresentados
> eram desenhados com a câmera do `OverrideView` anterior**.

### 5.8.1 Leia a câmera REAL da tela, não a câmera do engine

⚠️ **O hook do engine te dá a câmera do frame *sendo preparado*. A câmera na tela é outra.**

A técnica que resolve: **leia a view-projection direto dos constantes do vertex shader.** Com ReShade
add-on você tem `push_constants`; a matriz está em `c8..c11`.

```
matriz de view-projection nos push constants = a câmera que o host realmente usou neste pixel
```

Depois: mantenha um histórico das câmeras que você hookou e case a matriz lida com a entrada
correspondente. Isso é **agnóstico de engine** — funciona em qualquer host que exponha constantes
de shader.

⚠️ Se seu host não tem ReShade: procure a mesma matriz num constant buffer via **RenderDoc**, ou
aplique o preditor do §5.8.2 mesmo sem ela (é um EMA, não precisa da matriz).

### 5.8.2 O preditor

Não compense o atraso — **preveja** e corrija:

```
agora            = tempo do frame em tela
renderizado_para = tempo da câmera com que o convidado renderizou
atraso           = agora - renderizado_para          ← EMA, não o valor bruto
câmera_alvo      = predição da câmera em (renderizado_para + atraso)
→ mande o convidado renderizar para câmera_alvo
→ no composite, reprojete para a câmera que está NA TELA
```

⚠️ **A chave:** a câmera que você manda para o convidado é *prevista*; a câmera que você
reprojeta para é *a que está na tela*. São duas câmeras diferentes. Confundi-las é o bug clássico.

### 5.8.3 O algoritmo de reprojeção (o detalhe caro)

⚠️ **Não comece o ray-march no pixel "no infinito".** Onde esse raio não bate em nada — e as
paredes do host geralmente não estão na profundidade do convidado — a busca **para**, e você perde
uma faixa larga quanto a paralaxe. ⚠️ *"Uma marcha de 16 passos em 1/profundidade ainda pulava
para qualquer coisa além de ~6 blocos."*

**O algoritmo que funciona, em três passos:**

```
1. PASAGEM DE TILE (coarse, 1/16 da resolução)
   Em cada tile, guarde a PROFUNDIDADE MÍNIMA do conteúdo do convidado.
   Isso limita quão perto qualquer coisa no caminho do raio pode estar.

2. PASSEIO NEAR-TO-FAR (≈1 px por passo)
   Dado o raio e a profundidade-mínima do tile, ande de perto para longe em passos de ~1 px
   e pegue o PRIMEIRO cruzamento.
   → começar perto, não longe, é o que garante que a busca não para no vazio.

3. TESTE DE ESPESSURA
   Depois do cruzamento, teste se você atravessou algo "atrás de um contorno" e descarte.
   Sem isso, reprojeção acha bordas e vira serrilhado.
```

⚠️ **O overscan é obrigatório com predição.** Erro de predição empurra o picture para fora da
borda. Solução: o convidado renderiza com **`tan(fov/2) × 1.08`** (8 % de overscan); na amostragem,
**cor em bilinear, profundidade em point** (interpolar profundidade cria halos).

⚠️ **Entrada do jogador precisa ser vista.** Fazer raycast a partir da **câmera**, não do olho, em
terceira pessoa — a câmera do host está deslocada do olho do jogador.

### 5.8.4 Testes que provam que a reprojeção está certa

Não confie no olho. Meça:

- **Pilar dourado**: um objeto só-do-convidado numa profundidade conhecida, contra um fundo
  só-do-host. A posição projetada analiticamente e a position observada devem bater dentro de
  **~1 px**
- **Profundidade vs raycast nativo**: a profundidade que você lê do frame **tem de ser igual** à
  que o raycast do próprio host devolve. Medido: `5.5000` vs `5.5000` blocos. Zero diferença
- **Strafe e giro**: ande em zigue-zague e gire rápido. Contorno cortado de um lado = §5.8.3
  quebrado

# FASE 6 — CAMADA 4: JANELA E INPUT

## 6.1 Overlay de janela (modo A)

```
GWL_STYLE    → WS_POPUP | WS_VISIBLE
GWL_EXSTYLE  → WS_EX_LAYERED | WS_EX_TRANSPARENT | WS_EX_TOPMOST
             | WS_EX_NOACTIVATE | WS_EX_TOOLWINDOW
SetLayeredWindowAttributes(hwnd, 0, 255, LWA_ALPHA)
SetWindowPos(hwnd, HWND_TOPMOST, x, y, w, h, SWP_NOACTIVATE|SWP_FRAMECHANGED|SWP_SHOWWINDOW)
SetForegroundWindow(minecraftHwnd)      ← o INPUT VAI PARA O MINECRAFT
```

## 6.2 Achar as janelas

O PID do Minecraft vem do próprio shm (offset 44):

```csharp
static IntPtr FindWindowOf(int pid, Func<string,string,bool> accept) {
    IntPtr found = IntPtr.Zero;
    EnumWindows((h, _) => {
        GetWindowThreadProcessId(h, out var p);
        if (p != pid || !IsWindowVisible(h)) return true;
        var sb = new StringBuilder(256); GetWindowTextW(h, sb, 256);
        var cb = new StringBuilder(256); GetClassNameW(h, cb, 256);
        if (accept(sb.ToString(), cb.ToString())) { found = h; return false; }
        return true;
    }, IntPtr.Zero);
    return found;
}
var uk = FindWindowOf(Process.GetCurrentProcess().Id, (t,c) => c == "UnityWndClass");
var mc = FindWindowOf(frames.McPid,                   (t,c) => t.StartsWith("Minecraft"));
```

## 6.3 Keep-alive + windowed

```csharp
if (IsIconic(mc)) return;
if ((GetWindowLongPtr(uk, GWL_STYLE) & WS_MINIMIZE) != 0 || !HasFlag(WS_VISIBLE)) ReStyle();
GetClientRect(mc, out var r); ClientToScreen(mc, out var p);
SetWindowPos(uk, HWND_TOPMOST, p.X, p.Y, r.right, r.bottom, 112);
```

⚠️ **Force windowed antes de tudo:**
```csharp
if (Screen.fullScreenMode != FullScreenMode.Windowed) {
    savedMode = Screen.fullScreenMode;
    Screen.fullScreenMode = FullScreenMode.Windowed;
    return false;   // tenta de novo no tick seguinte
}
```
Fullscreen exclusivo não tem `WS_EX_TRANSPARENT` funcional. É a causa nº1 de "o overlay não aparece".

## 6.4 Teclas globais

```csharp
[DllImport("user32.dll")] static extern short GetAsyncKeyState(int vKey);
static bool Tapped(int vk, ref bool wasDown) {
    bool down = (GetAsyncKeyState(vk) & 0x8000) != 0;
    bool edge = down && !wasDown; wasDown = down; return edge;
}
```

## 6.5 Injetar input no convidado (modo B)

Quando o **host** tem o foco, injete no **SDL** — onde o Minecraft realmente lê:

```java
case IN_KEY(1):
    key(handle, code, a != 0); updateModifiers();
    int action = down && !wasDown ? 1 : (down && wasDown ? -1 : 0);
    int keycode = SDLKeyboard.SDL_GetKeyFromScancode(scancode, (short)modifiers, true);
    keyboardHandler.keyPress(handle, action, new KeyEvent(scancode, keycode, modifiers));
case IN_MOUSE_BUTTON(2): mouseHandler.onButton(handle, new MouseButtonInfo(code, modifiers), a != 0 ? 1 : 0);
case IN_SCROLL(3):      mouseHandler.onScroll(handle, 0.0, a / 120.0);   // WHEEL_DELTA
case IN_CURSOR(4):      mouseHandler.onMove(handle, a, b, dx, dy);
case IN_TEXT(5):        if (gui.screen() != null)
                            keyboardHandler.textInput(handle, new String(Character.toChars(a)));
case IN_RELEASE_ALL(6): releaseAll();
case IN_OPEN_MENU(8):   if (gui.screen() == null) { releaseAll(); gui.setScreen(new PauseScreen(true)); }
```

E o mixin que faz **todo** o polling de tecla do MC ler o host:

```java
@Inject(method = "isKeyDown", at = @At("HEAD"), cancellable = true)
static void keyDown(InputConstants self, int key, CallbackInfoReturnable<Boolean> cir) {
    if (SkyClient.tookOver()) cir.setReturnValue(InputBridge.isKeyDown(key));
}
// grabMouse / releaseMouse: cancelados enquanto tookOver
```

⚠️ **`RELEASE_ALL` não é opcional.** Sem ele, as teclas ficam presas quando o host ganha foco. Chame em:
`IN_RELEASE_ALL`, `menuOpen`, `loading`, queda de link, e antes de abrir a pausa.

⚠️ **Corrija a foco:**
```java
@Inject(method = "isFocused",  at = @At("HEAD"), cancellable = true) → linked() quando tookOver
@Inject(method = "isIconified", at = @At("HEAD"), cancellable = true) → false enquanto linked
// Screen: isPauseScreen → false enquanto linked
```
Se o MC achar que perdeu o foco, ele pausa e você perde o mundo.

## 6.6 Overlay vs injeção

| | Overlay | Injeção |
|---|---|---|
| Quem tem o foco | Minecraft | Host |
| Esforço | baixo (Win32 puro) | alto (hook no input stack) |
| Risco | estilo não aplica | teclas presas, pausa acidental |
| Quem faz | ULTRAKILL, GTA | SkyCraft |

Overlay resolve 80% dos casos.

---

# FASE 7 — COLISÃO

## 7.1 A regra

**Não substitua o sistema de colisão. Adicione um provedor ao lado do original.**

`VoxelShape` é uma abstração: qualquer coisa que responda `collide(Axis, AABB, distance)` pode ser
empurrada na lista.

```java
// EntityCollideMixin — um shape EXTRA
List<VoxelShape> shapes = original.call(level, source, area);
TriShape tris = UkGeometry.collides(source) ? TriShape.of(area) : null;
if (tris == null) return shapes;
List<VoxelShape> out = new ArrayList<>(shapes.size() + 1);
out.addAll(shapes);
out.add(tris);              // ← anexado, não substituto
return out;
```

Todo o movimento varrido nativo (sub-passos, step-height, chão) continua sem modificação.

## 7.2 Broad phase (dois níveis)

```
Nível 1 — mundo, célula de 16 blocos
  Long2ObjectOpenHashMap<int[]>   célula → peças
  > 64 células por peça  →  lista "big" (sempre testada por AABB)
  query > 512 células   →  só "big"
Nível 2 — por peça, dentro da malha
  count <= 16  →  varredura linear
  senão        →  grade uniforme, célula = max((hi−lo)/24, 0.001)
  query > 4096 células  →  linear
```

Os bail-outs são **deliberados**: melhor conservador e lento num caso raro do que grade gigante.

## 7.3 Formas parciais (onde a geometria real não é legível)

Algoritmo por célula:
1. `OverlapBox` — sem hit → vazio
2. **5 raios para baixo** (centro + 4 cantos a ±0.45), aceita `Dot(normal, up) > 0.3` → código
   **positivo** 1..16 = laje de piso de `n/16`
3. **5 raios para cima**, aceita `Dot(normal, up) < −0.3` → **negativo** −1..−15 = laje de teto
4. Nenhum → 16 (cheio)

Range: **−115..116**. `+100` = "duro" (malha ilegível), nunca cede a triângulos.

⚠️ **Codificação alternativa do SkyCraft** (mais compacta, para quando a geometria existe mas você quer
colisão blocada barata para mobs):
```
FILL = bits 0-9 : contagem de voxels sólidos
       bit 10    : geometria na metade de baixo
       bit 11    : geometria na metade de cima
       bits 12-14: índice da camada não-vazia mais alta
groundTop(pos) = ((fill >> 12 & 7) + 1) / 8f
```

## 7.4 SAT dentro do motor vanilla

**13 eixos** — verificado no fonte:

```
1–3:  eixos da caixa      (1,0,0) (0,1,0) (0,0,1)
4:    normal do triângulo  n = e0 × e1
5–13: cada uma das 3 arestas e: (0,−e.z,e.y) (e.z,0,−e.x) (−e.y,e.x,0)
```

```java
private static boolean axis(double lx,double ly,double lz,int a,
                            double cx,double cy,double cz,double hx,double hy,double hz,
                            double v0x,double v0y,double v0z,
                            double v1x,double v1y,double v1z,
                            double v2x,double v2y,double v2z, double[] lo,double[] hi) {
    double len = Math.sqrt(lx*lx + ly*ly + lz*lz);
    if (len < 1.0E-12) return true;               // eixo degenerado: passa
    lx /= len; ly /= len; lz /= len;
    double t0 = lx*v0x+ly*v0y+lz*v0z, t1 = lx*v1x+ly*v1y+lz*v1z, t2 = lx*v2x+ly*v2y+lz*v2z;
    double tmin = Math.min(t0, Math.min(t1, t2));
    double tmax = Math.max(t0, Math.max(t1, t2));
    double r = hx*Math.abs(lx) + hy*Math.abs(ly) + hz*Math.abs(lz);
    double c = lx*cx + ly*cy + lz*cz;
    double from = tmin - r + 1.0E-7 - c;          // EPS encolhe
    double to   = tmax + r + 1.0E-7 - c;
    if (from >= to) return false;                 // eixo separador
    double la = a == 0 ? lx : (a == 1 ? ly : lz);
    if (Math.abs(la) < 1.0E-12) return from < 0.0 && 0.0 < to;
    double s0 = from / la, s1 = to / la;
    if (la < 0.0) { double t = s0; s0 = s1; s1 = t; }
    if (s0 > lo[0]) lo[0] = s0;
    if (s1 < hi[0]) hi[0] = s1;
    return lo[0] < hi[0];
}
```

⚠️ **A política é deliberadamente tolerante** — é isso que faz rampa parecer lisa:

```java
if (d > 0.0) {
    if (s1 <= 0.0 || s0 >= d)  return d;
    if (s0 < 0.0)  return -s0 < 0.02 ? 0.0 : d;   // já dentro: só trava se raspou
    return Math.min(d, s0);
} else {
    if (s0 >= 0.0 || s1 <= d)  return d;
    if (s1 > 0.0)  return s1 < 0.02 ? 0.0 : d;
    return Math.max(d, s1);
}
```

`PENETRATION = 0.02`, `EPS = 1.0E-7`. Devolve **distância limitada**, não MTV real.

**`getCoords(Axis.Y)` — o detalhe que dá chão correto em rampas:** recorte **Sutherland–Hodgman** de
cada triângulo contra as 4 faces laterais da região, toma o `maxY` do polígono recortado, ordena.

```java
if (heights == null) {
    for (int o = 0; o < tris.length; o += 9) {
        double top = clippedTop(tris, o, region);
        if (top >= region.minY && top <= region.maxY) out.add(top);
    }
    out.sort(null); heights = out;
}
```

⚠️ **Empurra-pra-fora (SkyCraft):** Sutherland–Hodgman duas vezes (plano baixo e alto), depois ponto
mais próximo por ponto-em-polígono por área assinada, e aplica **só a penetração mais profunda**, uma
vez. Sem isso, encostado em duas paredes, o jogador vibra entre elas.

⚠️ **`walkable` por normal:** `|ny| >= 0.7`. Piso só começa a bloquear **acima do step height**
(`wallFrom = wasOnGround ? step : 0.02`), senão você não sobe degraus. Descida em rampa é
proporcional à velocidade: `y − floorWalk <= max(step, horizontal × 1.5)`.

**Esterilize o resto da abstração:** `toAabbs() = List.of()`, `forAllBoxes/forAllEdges` vazios,
`clip() = null`, `closestPointTo = Optional.empty()`, `getFaceShape = Shapes.empty()`,
`move/optimize/singleEncompassing = this`.

## 7.5 Faça o bloco ceder

```java
@Override
protected VoxelShape getCollisionShape(BlockState state, BlockGetter level,
                                       BlockPos pos, CollisionContext context) {
    return !state.getValue(HARD)
        && context instanceof EntityCollisionContext ec && ec.getEntity() != null
        && UkGeometry.collides(ec.getEntity())
        && UkGeometry.covers(pos)          // AABB do bloco + 0.1 casa com algum tri
      ? Shapes.empty()                    // ← a geometria real assume
      : super.getCollisionShape(state, level, pos, context);
}
@Inject(method = "destroyBlock", at = @At("HEAD"), cancellable = true)
static void protect(ServerPlayerGameMode self, BlockPos pos, CallbackInfoReturnable<Boolean> cir) {
    if (WorldBridge.isHostBarrier(pos) || state.is(BARRIER) || state.is(UkSolid.BLOCK))
        cir.setReturnValue(false);        // constrói em cima, mas não cava
}
```

## 7.6 Empurrar de volta (construções → colisão do host)

```
host: "blocks" {set:[x,y,z,...], clear:[x,y,z,...]}
      → GameObject com BoxCollider(size = Vector3.one * Map.S), isStatic
      → NavMeshObstacle { carving = true, carveOnlyStationary = true }   ← por isso isStatic
```

⚠️ `carveOnlyStationary = true` **exige** `isStatic = true`. É uma linha que custa uma hora de "por
que o inimigo atravessa minha parede".

⚠️ **Alternativa mais esperta (SkyCraft):** reescreva o **pathfinding do ator**.
`PathAvoid::Install` hooka o path solver (`SetupPathHook::thunk`) e passa um `AvoidArray` de pontos
onde não se pode andar. Funciona com paths que ignoram colliders (NavMesh dinâmico, stuck detection, AI
que teleporta quando trava).

## 7.7 Água (o SkyCraft foi além de todos)

**Render:** dobre a superfície do fluido para sentar sobre o terreno real:
```java
if (fluidGround > 0.0F) {
    float t = Math.clamp(y - fluidBaseY, 0, 1);
    y = fluidBaseY + fluidGround + t * (1.0F - fluidGround);
}
```

**Física:** grade 16×16 de alturas de superfície, substituindo o fluido em 4 pontos (`hasFluidAndLoaded`,
`getFluidState`, `FluidState.getHeight`, `getHeightForCamera`).

**Sentido inverso:** a água do MC **recusa fluir** para a geometria do host:
```java
@Inject(method = "canPassThroughWall", at = @At("HEAD"), cancellable = true)
static boolean refuse(BlockGetter level, FluidState state, BlockGetter target, BlockPos pos,
                      Direction direction, FluidState targetState, CallbackInfoReturnable<Boolean> ci) {
    if (!targetState.isAir() || direction == Direction.UP) return;   // só host, ar, não pra cima
    if (!SkyCollision.isKnown(pos)) { ci.setReturnValue(false); return; }   // ⚠️ região desconhecida
    if (direction == Direction.DOWN) {
        if (SkyCollision.hasGeometry(state, pos)) { ci.setReturnValue(false); return; }
        if (SkyCollision.groundTop(pos) >= 0.8388889) { ci.setReturnValue(false); return; }
    }
}
```

⚠️ **A primeira regra é a que importa: região desconhecida → recusa.** Sem isso a água escorre para fora
do mapa antes de você ter streaming de geometria.

---

# FASE 8 — COMBATE BIDIRECIONAL

## 8.1 Host → Minecraft

Um prefix num método de dano:
```csharp
[HarmonyPatch(typeof(NewMovement), "GetHurt")]
static class HurtPatch {
    static bool Prefix(int damage, bool instablack, bool explosion) {
        if (!Plugin.Active) return true;
        if (instablack || damage >= 1000) return true;   // ⚠️ nunca roube morte de cutscene
        Send(new { t = "hurt", d = damage * DamageScale });
        return false;
    }
}
```

## 8.2 Minecraft → Host

Envie estado, não colisão. O SkyCraft faz `EV_HIT_ACTOR` com flags de
`HIT_CRITICAL/PROJECTILE/SWEEP/FIRE` e classe de arma (`BLADE/AXE/BLUNT/PIERCE/ARROW`), e o plugin
resolve a pipeline de hit de verdade.

⚠️ **O proxy tem que ser vulnerável com dano cancelado:**
```java
public void tick() { baseTick(); setHealth(getMaxHealth()); }   // ⚠️ vida cheia permanente
@Override protected void actuallyHurt(...) { pendingDamage += dmg; /* ... */ }
@Mixin(LivingEntity.class)
@Inject(method = "hurtServer", at = @At("HEAD"), cancellable = true)
static void proxyNeverDies(LivingEntity self, ServerLevel level, DamageSource src, float amt,
                           CallbackInfoReturnable<Boolean> cir) {
    if (MobWar.isProxy(self)) { MobWar.onProxyHit(self, src, amt); cir.setReturnValue(false); }
}
```
⚠️ Se você fizer **invencível**, nenhum *target goal* consegue mirar (entidades invencíveis são
intocáveis) e os mobs simplesmente ignoram.

⚠️ **O flag `Invisible` reseta no primeiro sync de rede.** Dê um efeito `INVISIBILITY` infinito e oculto.

⚠️ **Derivativo útil:** o SkyCraft treina **skills do Skyrim** com ações do Minecraft — bloquear um
golpe treina Block, forjar treina Smithing com `worth()` por material (netherite 150, diamond 80, iron
40…), armadura classifica pesado vs leve pelo prefixo do id. Conteúdo do convidado virando progressão
do host.

---

# FASE 9 — ORDEM DE CONSTRUÇÃO

> **Construa nesta ordem.** Cada passo é verificável isoladamente, **cada um produz algo visível**, e
> cada um tem um **GATE** que você não passa até ver o resultado.
>
> ⚠️ A tentação de pular para o passo 5 (a parte interessante) é exatamente o que faz o projeto travar
> na semana 2.

| # | Passo | Entrega | GATE — não avance sem |
|---|---|---|---|
| 1 | Link vazio | `hb` a cada 0,1 s | ver `connected to Minecraft at ws://127.0.0.1:25599` |
| 2 | Overlay de janela | host por cima do MC | ver o host em cima, clicar, **e o MC responder** |
| 3 | Simbiose de teste | `{"t":"steve",pos,yaw}` → label | ver números se moverem |
| 4 | Câmera | pose no shm, textura, câmera do host | ver o mundo do MC pela câmera do host, obedecendo o mouse |
| 5 | **Profundidade** | depth-mesh / shader | o MC **tem profundidade**: parede do host esconde bloco, bloco esconde host |
| 6 | Esconder o boneco | `forceRenderingOff` + duplicado | ver o boneco no lugar certo, ou nada |
| 7 | Colisão em blocos | grade simples | o jogador para de atravessar paredes |
| 8 | Geometria triangular | `tris`/`trism` + SAT | rampas lisas, paredes finas seguram |
| 9 | Gravidade e portais | `Map.Turn` + hold/release | atravessar **sem pausa perceptível** ← métrica de qualidade |
| 10 | Combate | dano nos dois sentidos | levar dano dos dois lados |
| 11 | Construções | `blocks` → colliders + navmesh | suas paredes param inimigos; inimigos sobem suas escadas |
| 12 | Polimento | estilo, som, menus, LAN | — |

⚠️ **Estimativa: 4–8 semanas de meio tempo**, Camadas 1–4 reaproveitadas em qualquer outro alvo. **Se for
seu primeiro projeto de modding, dobre.**

## Passo 0 (novo, antes do 1) — os oracles falsos

⚠️ **Não comece a composeder nada antes de ter um host falso.** O passthrough GTA foi
**majoritariamente construído antes do jogo de 125 GB terminar de instalar**, contra:

- `fakehost.py` — host falso com geometria conhecida; compõe os frames do MC sobre uma cena sintética
  + checkerboard + pilar vermelho "que só o host tem". O PNG mostra se alinhamento e oclusão estão
  certos.
- `fakegta.cpp` — a metade do GTA inteira **exceto as natives**: janela D3D11 com depth buffer
  reversed-Z e as convenções de câmera do GTA, com o compositor e o cliente WebSocket compilados dentro.

Isso transforma "instalei 125 GB e testar" em "roda um exe de 50 KB". **Custo: 1–2 dias. Retorno: tira
o gargalo do passo 5.**

## Passo 5 em detalhe — é o difícil

⚠️ **Escolha β ou γ na Fase 1 e não troque depois.** Se γ, o teste de profundidade da §0.6 **já foi
feito** — você está aqui porque deu gradiente.

**β (depth-mesh):**
```
1/z = iz0 + iz1 × depth
vértice = (colX/iz, rowY/iz, S/iz)     ← espaço de câmera
```
A divisão de perspectiva do host reproduz os pixels do MC exatamente, então o **Z-test normal** resolve
a oclusão. Quatro pares de coeficientes vêm de `(flags & 1) × (flags & 4)`:

| flags | `iz0` | `iz1` |
|---|---|---|
| `&1` set, `&4` set | `1/far` | `(far−near)/(near·far)` |
| `&1` set, `&4` clear | `1/near` | `−(far−near)/(near·far)` |
| `&1` clear, `&4` set | `1/far` | `2(far−near)/(2·near·far)` |
| `&1` clear, `&4` clear | `1/near` | `−2(far−near)/(2·near·far)` |

Três passos por banda de linha:
1. **Linearizar** com filtro de 3 taps (máximo dos vizinhos horizontais) — mata ruído de 1 pixel
2. **Dilatar** a linha em 1 e preencher buracos com o `hmax` dos vizinhos
3. **Emitir** 6 índices por quad, com **cull de descontinuidade** `hi − lo > 0.15·hi + 0.01` → pula

⚠️ **Paralelize por linha, nunca mais fino.** As três passagens são disjuntas por construção — a linha
`i` só escreve `invZ`/`verts`/`tris` da linha `i` — então `Parallel.For` sobre bandas é seguro sem
lock. Use `Bands = clamp(ProcessorCount − 2, 1, 8)`.

## Passo 9 em detalhe — hold/release

O problema: quando a colisão do entorno precisa ser reconstruída (entrou num level, virou gravidade,
atravessou portal), o jogador precisa ficar parado até a geometria existir — **senão cai**.

**A solução: congelar o Minecraft, não o jogador.**

```
host: hold on:true   → + geometria + grade; o MC fica INVENCÍVEL e travado
host: grade próxima pronta (192 células em torno)
host: hold on:false  → "collision ready around Steve (N cells): released after N ms"
```

```java
static void hold(LocalPlayer player) {
    if (released && HostState.attached()) { holdX = Double.NaN; Passthrough.playerHeld = false; }
    else {
        Passthrough.playerHeld = true;
        if (Double.isNaN(holdX)) { holdX = player.getX(); holdY = player.getY(); holdZ = player.getZ(); }
        if (holdY < player.level().getMinY() + 4) holdY = 100.0;   // rede no void
        player.setPos(holdX, holdY, holdZ);
        player.setDeltaMovement(Vec3.ZERO);
        player.fallDistance = 0.0;
    }
}
```

⚠️ **ack com timeout no host**, para nunca travar se um lado morrer:
```csharp
if (warpPending && (dist2 > 9.0 || now - warpSentAt > 1f)) { warpPending = false; }
```

**Teleporte sem borracha:**
```java
player.setPos(to.x, to.y, to.z);
player.xo = player.xOld = to.x - vel.x;   // ← a posição INTERPOLADA já nasce do lado certo
player.yo = player.yOld = to.y - vel.y;
player.zo = player.zOld = to.z - vel.z;
player.setDeltaMovement(vel);
player.fallDistance = 0.0;
```

**Anti-embedding:**
```java
if (index().overlaps(box.deflate(1e-4))) {
    for (double d = 0.03125; d <= 2.000000001; d += 0.03125)          // passos de 1/32
        for (Vector3 dir : FOURTEEN_DIRECTIONS)                        // 6 eixos + 4 diag + 4 + -y
            if (!level().noCollision(player, box.move(dir.scale(d)).deflate(1e-4))) {
                player.setPos(pos.add(dir.scale(d)));
                double into = v.dot(dir);
                if (into < 0.0) player.setDeltaMovement(v.subtract(dir.scale(into)));
                return;
            }
}
```
⚠️ O cancelamento do componente de velocidade **na direção da parede** é o que impede que ele seja
empurrado para fora e para dentro alternadamente a cada frame.

---

# FASE 10 — ORACLES (obrigatório por passo)

> Agentes (e humanos cansados) falham em modding **derivando**: construindo com convicção sobre um
> palpite errado. O remédio é um **oráculo** — algo mecânico que diz certo ou errado, rodado depois
> de cada mudança.

## 10.1 Os oráculos

| Oráculo | Pega | Exemplo real |
|---|---|---|
| **Round trip** | formato mal entendido | AoE2 SLD: decode→encode→decode, erro 0,91/255 |
| **Trace replay** | um port que diverge do jogo | Terraria EoC: estado frame-t + ação → comparar t+1 (99,9%) |
| **Cena roteirizada + screenshot que você OLHA** | escala, orientação, pivot, camada | comando de chat que invoca, screenshot |
| **Log do jogo** | erro de load, exceção, asset faltando | ver tabela abaixo |
| **Host sintético** | bug de integração antes do jogo existir | `fakehost.py`, `fakegta.cpp` |
| **Cena de medição** | timing e sync | parede dourada só-MC vs horizonte só-host |
| **Byte-match de build** | erro de decompilação | decomps que compilam de volta ao ROM idêntico |
| **Publish check** | enviar o que não deve | `um publish check` |

**Logs:**

| Jogo / loader | Log |
|---|---|
| BepInEx | `BepInEx/LogOutput.log` |
| Unity | `Player.log` em `AppData/LocalLow/<company>/<product>/` |
| UE4SS | `UE4SS.log` |
| SKSE | `Documents/My Games/<game>/SKSE/` |
| tModLoader | `client.log` |
| **Minecraft** | `<gameDir>/logs/latest.log` |

## 10.2 As quatro regras

1. **Automatize o loop inteiro** — launch → menus → cena → check → log — para que **um comando**
   responda "funcionou?".
2. **Circuit breaker:** depois de ~3 falhas idênticas, **pare**, escreva o que sabe, mude de abordagem.
3. **Um diário (`MODLOG.md`):** todo fato confirmado, e cada beco sem saída **com o porquê**. Sobrevive a
   compactação de contexto.
4. **Seja honesto no resultado:** escreva o que o oráculo **não** cobriu.

## 10.3 ⚠️ Os oráculos que mentem

**1. O oráculo congelado.** Uma captura via `Windows.Graphics.Capture` **deixa de rastrear** a janela
assim que o swapchain passa por ReShade/independent flip, e **repassa o último quadro composto** —
byte-idêntico, SHA igual, mesmo com o processo claramente renderizando (7,2 s de CPU por 5 s de wall
clock).

> **O que isso inverte:** o oráculo diria "o shader não está rodando" quando a verdade é "o shader roda
> perfeitamente e a profundidade que ele lê está vazia." Foi literalmente o caso do Wukong.

**Como não cair:** **prove que o oráculo está vivo antes de confiar nele.** Duas capturas com 1 s de
intervalo, compare hashes; se batem enquanto o jogo anima, o oráculo está morto. Use uma captura de
**dentro** do que você mede (o `Print Screen` do ReShade escreve o quadro pós-processado ao lado do DLL).

⚠️ E o inverso: uma cena legitimamente estática faz duas capturas iguais. Confira o processo queimando
CPU.

**2. Screenshots que ninguém olha.** O agente salva mas nunca abre. Abra uma cópia reduzida depois de
cada mudança visual. É a única forma de pegar "virado para a esquerda em vez da direita".

**3. Resultado negativo sem verificar o loop.** Um scan em chunks reportou "nenhuma string" porque o
loop usava `$exe.Length` sobre a **string do caminho** (~100), não o arquivo. Use
`(Get-Item $path).Length` e guarde o loop.

**4. "Funciona no host falso" ≠ "funciona no jogo."** O jogo real adiciona pausa, câmera ociosa, foco de
janela, protetor de tela. Rode o jogo real antes de dizer que acabou.

**5. Oráculo que testa o frame errado.** Off-by-one: a ação do frame t aparece em t+1, e um script lê a
câmera do frame **sendo preparado**. Meça com um teste deliberado antes de confiar no replay.

**6. `PYTHONUTF8=1`.** `um win ps` crasha em Windows de locale não-UTF8 (`UnicodeDecodeError: 'gbk'`)
porque títulos de janela têm bytes que o locale não mapeia. Antes de `um win ps/shot/drive`.

**7. DPI.** `um win drive click` recebe coordenadas **virtualizadas**; `um win shot` retorna pixels
**físicos**. Em 150%: `click = screenshot_px / 1.5`.

---

### 10.4 Oracles numéricos — os que não mentem por opinião

⚠️ **"parece certo" não é oracle.** Estes quatro dão número, e número se compara:

| Oracle | Como | Aceitação |
|---|---|---|
| **Projeção analítica** | um objeto de profundidade conhecida, projetado na mão, vs. onde ele aparece | **dentro de ~1 px** |
| **Profundidade vs raycast nativo** | profundidade do frame vs. o raycast do próprio host no mesmo ponto | **exato** (ex.: `5.5000` vs `5.5000` blocos) |
| **Cena de medição** | objeto só-do-convidado contra fundo só-do-host | desloca junto, sem faixa |
| **Contagem de entidades** | quantas coisas atravessaram, aterrissaram, tomaram dano | bate com o roteiro do teste |

⚠️ **O quarto é o que impede falso positivo em multiplayer-like.** Documentado no Portalcraft: os
logs mostravam *"entidades carregadas através de portais, aterrissagens (altura, velocidade,
dano), dano chegando ao Portal 2 (`fall took 30.0 health`), curas, conquistas, mortes por goo"*.
Isso é uma lista de afirmações verificáveis, não uma impressão.

⚠️ E **hook de teste com undo obrigatório**: *"todo hook de teste que muda o mundo tem um
desfazer, e testes de campanha em Survival disparam recompensas reais: desfaça-as."* Senão o
usuário volta e encontrou o mundo dele diferente.

# 11. ARMADILHAS CONSOLIDADAS

Todas verificadas em código decompilado ou nos binários das três implementações.

## 11.1 Arquitetura

1. **Nav desliga quando o frame vira.** `Nav.Rebuild`/`LinkBlocks` abortam em `Map.Turned`, e
   `Nav.Near` usa `Quaternion.identity` apesar dos blocos serem posicionados com `Map.Frame` — seguro
   *só porque nunca roda nesse estado*. Qualquer refatoração que remova o `Map.Turned` quebra, e a causa
   vai parecer não ter nada a ver com navmesh.
2. **`ground` é código morto** no ULTRAKILL 0.2.0. Apague caminhos mortos ao portar entre versões.
3. **Herança do GTA V.** As mensagens `gta`/`director` existem porque o ULTRAKILL é derivado daquele
   exemplo. Você não precisa — mas se mudar, mantenha o fan-out.
4. **Settings capturados no `Awake()`.** Mudar `UnitsPerBlock` em runtime desalinha a geometria
   ("Steve flutua 2 unidades"). Registre handler de mudança ou documente que exige restart.

## 11.2 Payload e threading

5. **`tris.d` é float32, não double.** Só `trism` é double (passo 13).
6. **`List<double>` recebendo um `int`.** `moves.Add(p.Id)` — funciona, mas é um tipo errado esperando
   um bug. Cast explícito.
7. **Reatribuição de vértices todo frame.** `mesh.vertices = verts` sempre, mesmo sem mudança. Só o
   índice é compactado. Se o custo aparecer, é aqui.
8. **Stats que colapsam.** `"moved most: name xN"` indexa por `Collider.name`; 300 peças com o mesmo
   nome viram uma entrada. Seu log de perfilagem mente.
9. **Bug de wrap no anel binário** (§3.6).
10. **Orçamento precisa de contagem E tempo.** O SkyCraft usa `12 seções OU 3 ms`, o que quer que venha
    primeiro. Contagem sozinha não protege de uma seção patológica.

## 11.3 Engine

11. **ReShade precisa carregar como ASI em alguns jogos.** O GTA carrega o `dxgi.dll` de sistema antes
    de um proxy na pasta dele.
12. **Profundidade pode estar inacessível.** Wukong/UE5/D3D12: hooka, detecta o depth-stencil, lê
    profundidade **plana**. (§0.6, §10.3)
13. **Teclas presas** quando o host ganha foco. `releaseAll()` em menu/loading/queda de link.
14. **O convidado auto-pausa** ao "perder" o foco → perde-se o mundo. Force `isFocused`/
    `isIconified`/`isPauseScreen`.
15. **`player_movement_check`** briga com teleporte constante. Force `false`.
16. **Elytra: "deslizar" é queda livre.** Com `noPhysics`, `onGround` nunca atualiza e o servidor cancela
    todo planador. Force `setOnGround(false)`. Remova a elytra no join.
17. **Proxies ficam visíveis.** O sync de rede reseta `Invisible` sem efeito. Efeito `INVISIBILITY`
    infinito.
18. **Mobs ignoram proxies.** Invencível = intocável = sem alvo. Deixe **vulnerável** e cancele o dano.
19. **⚠️ F3D3D `ReadBackbuffer` vaza** 17 GB num take (Terraria/FNA). **Nunca** leia o back buffer de
    dentro do jogo. Capture a janela de fora (ffmpeg `gfxcapture=hwnd=...`).
20. **Nunca bloqueie a thread principal** esperando ffmpeg parar. Mande `'q'` e espere off-thread.
21. **Escreva `.mkv` enquanto grava:** sobrevive a um kill; mp4 perde o índice.
22. **Paks/recursos na versão errada** crasham no mount.
23. **Atualização de jogo move tudo.** Fixe a versão e **documente qual build você suporta**.

## 11.4 Settings do Minecraft (que todo passthrough sobrescreve)

O mod **precisa** forçar: sem nuvens, sem bob de visão, sem vinheta, FOV effect 0, cap de fps,
pause-off, spawn de mobs off, `time set noon`, `player_movement_check false`, `keep_inventory true`.

⚠️ Isso é **necessário** e **destrutivo** para a experiência padrão. **Obrigatório: game dir separado.**
Os três fazem isso (ULTRAKILL cria um mundo void; GTA um `passthrough`; SkyCraft um preset `mirror`).

## 11.5 Reprojection / timing

24. **Um frame de lead.** O script lê a câmera do frame *sendo preparado*. Re_projetar para a pose
    **anterior**. Meça com a cena de parede dourada.
25. **`GetTickCount` judder.** Passos de 16 ms. Use `nanoTime`/QPC.
26. **Escala de resolução interna ≠ swapchain.** Wukong renderiza 1708×1068 com swapchain 2560×1600.
    O composite tem que casar com a resolução **interna**.

## 11.5b Composição, entrada e multiplayer — as que custam mais caro

⚠️ Todas medidas, todas com a mesma forma: **o sintoma parece um de sistema, a causa é de
sincronismo.**

32. **A câmera na tela é quase sempre a anterior.** Com as configurações de um usuário real, **70–80 %
   dos frames** foram desenhados com a câmera do hook anterior. See §5.8.
33. **O contorno dos mobs é cortado de um lado ao strafar.** A busca de reprojeção começou no pixel
   "no infinito" e parou onde não bateu nada — perda de uma faixa larga quanto a paralaxe. Uma
   marcha de 16 passos em 1/profundidade **ainda** pulava além de ~6 blocos. See §5.8.3.
34. **O contorno é cortado nas bordas ao girar.** Erro de predição empurra o picture para fora.
   Overscan de `tan(fov/2) × 1.08`.
35. **Engolir `WM_*BUTTON*` não bloqueia o botão.** O engine recebe os botões por outro caminho.
   ⚠️ *Limpe os flags de botão no hook de geração de input* (`IN_ATTACK`/`IN_ATTACK2` num
   `CreateMove`), não na camada de OS.
36. **Escrever a vida direto quebra os frames de invulnerabilidade.** ⚠️ *"lava machucou 20 vezes
   por segundo — a vida era resetada todo tick, então os i-frames do Minecraft nunca aplicavam."*
   **Deixe o vanilla computar o dano, mande a vida que ele tirou, espelhe a vida de volta.**
37. **Teletransportar uma entidade a faz "deslizar".** `teleport` é interpolado pelo cliente.
   **Recrie a entidade na saída** — como viagem entre dimensões faz: restaure do backup,
   remova a antiga **primeiro** para o mesmo UUID poder ser adicionado de novo.
38. **`noPhysics` deve ser escopado, não permanente.** Com ele sempre ligado, o servidor colide o
   stand-in com barreiras que o jogador do host atravessa (portais, vãos). ⚠️ *"`noPhysics` só
   enquanto o servidor processa pacotes de movimento."*
39. **A tecla que abriu o menu conta como pressionada nova dentro dele.** ⚠️ *Ignore teclas que já
   estavam seguradas quando um menu abriu, até serem soltas.* (É o mesmo caso do `RELEASE_ALL` do
   SkyCraft, mais específico.)
40. **A roda do mouse não scrolla se você normaliza duas vezes.** O delta do ReShade **já está em
   notches**. Dividir por 120 de novo zera.
41. **"Sem dano de queda" tem duas causas.** A fórmula de queda retorna cedo para quem está voando
   (o stand-in sempre voa), **e** a gamerule estava off. ⚠️ **Compute a fórmula do convidado a
   partir da aterrissagem do host** (altura + velocidade de impacto, para que deslizes lentos não
   contem) e aplique com a fonte de queda correta.
42. **O watchdog não pode usar heartbeat de frame.** Jogo minimizado não produz frame. ⚠️ *Verifique
   que o processo existe.*
43. **Nome de usuário aleatório = reset de tudo.** UUID novo a cada lançamento. Fixe `--username`.
44. **Sem hash não é "nada a buscar".** Um profile meta dava `sha1`/`size` para tudo **exceto** o
   próprio loader, e o instalador leu a ausência como "não precisa baixar". ⚠️ *Quando não há hash,
   baixe `<artifact url>.sha1` e verifique.*

### 11.5c Geometria de level → grade de voxels

45. **Voxelize só a casca de superfície.** Ao converter brushes para blocos de colisão, ⚠️ *apenas a
    casca de superfície vira bloco* — o volume sólido viraennything, e o interior não precisa.
46. **Deslocamento por mapa para alinhar.** ⚠️ *Um shift por mapa põe a maioria dos pisos em
    fronteiras de bloco exatas.* Sem isso, metade dos pisos fica meio bloco acima ou abaixo.
47. **Brushes não cobrem tudo: props estáticos faltam.** Trez Camadas:
    - **brushes/lumps** (a geometria do level) → voxelize
    - **trace de raycast** com filtro "que atinge nada" → acha props e portas (pisos por
      trace para baixo, paredes por teste pontual na altura do mob)
    - o resto → não espelha, e documente
48. **Override a `dimension_type` para o mundo ficar alto o bastante.** O nível de origem pode ter
    mais escala vertical do que o padrão do convidado.

## 11.6 Epistemologia — as que mais importam

27. **O oráculo congelado.** (§10.3)
28. **Screenshot que ninguém olha.** (§10.3)
29. **Resultado negativo sem verificar o loop.** (§10.3)
30. **"Funciona no falso" ≠ "funciona no real".** (§10.3)
31. ⚠️ **O mais importante:** o beco do Wukong foi encontrado **de propósito, antes** de escrever
    qualquer código de composição. O recon do depth foi *front-loaded*. Se você for usar γ, essa é a
    **primeira** tarefa, não a última.

---

# 12. CHECKLIST POR JOGO (preenchível)

> Copie, preencha para o jogo em questão, e mantenha no `MODLOG.md`.

## Recon
- [ ] Jogo: `<versão/build>` — Steam appid `<id>`
- [ ] Engine: `<engine>` `<versão>` — loader: `<qual>` `<versão>`
- [ ] Anti-cheat: `<qual>` — modo de teste: `<como>`
- [ ] Logs: host `<path>` · MC `<gameDir>/logs/latest.log`
- [ ] Saves: `<pasta>` — backup feito: `<quando>`
- [ ] A1 profundidade: `<resposta + evidência>`
- [ ] A2 câmera: `<resposta>`
- [ ] A3 colisão: `<recurso/formato>`
- [ ] A4 dano: `<método>`
- [ ] Gate de profundidade (se γ): `<resultado medido>`
- [ ] Gate de segurança (§0.4): passou / **parou**

## Arquitetura
- [ ] Câmera: `<A|B|C>` — `<razão>`
- [ ] Render: `<α|β|γ|δ>` — `<razão>`
- [ ] Transporte: `<JSON+shm | shm binário | shm+LAN>`
- [ ] `UNITS_PER_BLOCK = <medido>`
- [ ] Tabela de ângulos: `<derivada>`
- [ ] §2 do playbook aplicável: `<qual engine>`

## Camadas
- [ ] Link + heartbeat + PID no shm
- [ ] ABI + ring de 3 + 3 fences + watchdog
- [ ] Overlay/injeção de janela
- [ ] Protocolo + tabelas
- [ ] Seqlocks / rings
- [ ] `Map` afim + `Version`
- [ ] Colisão: SAT 13 eixos + Sutherland–Hodgman + bloco que cede
- [ ] Combate nos dois sentidos

## Os 12 passos (com o gate de cada um)
1. [ ] Link — `GATE:` ver `connected to Minecraft`
2. [ ] Overlay — `GATE:` clicar responde no MC
3. [ ] Simbiose — `GATE:` números se movem
4. [ ] Câmera — `GATE:` câmera obedece o mouse
5. [ ] Profundidade — `GATE:` oclusão correta
6. [ ] Boneco — `GATE:` boneco no lugar certo
7. [ ] Colisão blocada — `GATE:` não atravessa parede
8. [ ] Triângulos — `GATE:` rampa lisa
9. [ ] Gravidade/portal — `GATE:` **sem pausa perceptível**
10. [ ] Combate — `GATE:` dano dos dois lados
11. [ ] Construções — `GATE:` inimigos sobem suas escadas
12. [ ] Polimento

## Reprojeção (se γ ou δ com câmera prevista)
- [ ] Li a view-projection dos **constantes de shader**, não só do hook de engine
- [ ] Previsor com **EMA** de `mostrado - renderizado_para`
- [ ] Duas câmeras distintas: prevista (para o convidado) e na-tela (para o reprojetar)
- [ ] Passe de tile coarse (1/16) → **min-depth**
- [ ] Paseo near-to-far em ~1 px, com teste de espessura
- [ ] **Overscan** `tan(fov/2) × 1.08`; cor bilinear, profundidade point
- [ ] Raycast de input a partir da **câmera**, não do olho (3ª pessoa)

## Oracles numéricos (obrigatório)
- [ ] Projeção analítica vs. observada: **dentro de ~1 px**
- [ ] Profundidade do frame == raycast nativo: **exato**
- [ ] Contagem de entidades/aterrissagens/dano bate com o roteiro
- [ ] Hooks de teste com **undo** (inclusive recompensas de Survival)

## Ciclo de vida
- [ ] Lançador: proxy → convidado oculto → host → salva/encerra → **remove o proxy**
- [ ] Watchdog checa **o processo**, não o heartbeat de frame
- [ ] `--username` fixo
- [ ] Efeito colateral de `cheats`/vac/etc. documentado para o usuário

## Proxy DLL
- [ ] `dumpbin /imports` para ver **de qual pasta** o host carrega
- [ ] Proxy na **subpasta** certa (`bin`, `game/bin`)
- [ ] **Prova de que carregou** (log no `DllMain` ou arquivo marcador)
- [ ] Testado numa install limpa

## Dano e i-frames
- [ ] Deixa o vanilla computar o dano, **não** escreve vida direto
- [ ] Dano de queda recomputado da física do host (altura + velocidade de impacto)
- [ ] `noPhysics` só no escopo certo

## Oracles
- [ ] Host sintético existe (antes do passo 5)
- [ ] Oráculo de screenshot **provado vivo** (hashes diferentes com 1 s)
- [ ] Cena de medição de latência (parede dourada)
- [ ] `MODLOG.md` com fatos **e becos com o porquê**
- [ ] Circuit breaker definido (3 falhas → para)

## Entrega
- [ ] Game dir separado do MC
- [ ] `Uninstall` escrito
- [ ] `um publish check` passou
- [ ] Créditos de loaders/licenças
- [ ] **AI disclosure honesto**

---

# APÊNDICE A — NÚMEROS DE REFERÊNCIA

⚠️ **Não são intercambiáveis. Meça o seu.**

## ULTRAKILL

| Constante | Valor |
|---|---|
| Porta WebSocket | `25599` |
| shm nome / magic | `Local\MCPassthroughFrame` / `1414546253` (`0x5450434D`) |
| shm tamanho | `298 602 496` (284 MB) |
| header / slots / stride | `4096` / `3` / `99 532 800` |
| frame máximo | `3840 × 2160` (`33 177 600` B/plano) |
| watchdog de slot | `1 s` |
| **unidades do host por bloco** | **2** |
| escala do Steve | `0.95` (1,71 blocos, abaixo dos 1,75 do V1) |
| altura de passo | `1` (vanilla é 0,6) |
| grade de blocos | XZ ±14, Y −8..+12 = 17 661 células |
| células "prontas" | raio 3,5 → **192** |
| orçamento de blocos/frame | `2000` (≈9 frames por varredura) |
| raio de geometria | `40` blocos, varredura a cada `0,25 s` |
| colliders por varredura | `Collider[8192]` |
| passos de profundidade/px | `3` → ~1,3 M triângulos a 1080p |
| bandas de thread | `clamp(cores − 2, 1, 8)` |
| penetração / epsilon SAT | `0,02` / `1,0E-7` (eixo degenerado `1,0E-12`) |
| célula do broad phase | `16` blocos (mundo), `(hi−lo)/24` (por peça) |
| **bytes por triângulo** | **36** (9 × float32) |
| **bytes por peça movida** | **104** (13 × double) |
| dano host→MC / MC→host | `0,2` / `0,15` |

## SkyCraft

| Constante | Valor |
|---|---|
| shm nome / magic / versão | `Local\SkyCraft_v1` / `1129925459` / `10` |
| shm tamanho | `200 327 168` (~191 MB) |
| **unidades SKSE por bloco** | **70** |
| SKY_STATE / MC_STATE | `256` / `512` |
| WATER_GRID / OVERLAY_CTL | `1024` / `768` |
| INPUT_RING / ACTOR_TABLE / EVENT_RING | `4096` / `73 728` / `94 208` |
| WORLD_ENTITIES / COLLISION_RING | `114 688` / `131 072` |
| OVERLAY_PIXELS / RENDER_RING | `33 685 504` / `133 218 304` |
| input ring / event ring | 4096 × 16 B / 512 × 32 B |
| actor table / world entities | 256 × 64 B / 160 × 96 B |
| collision ring / render ring | 32 MB / 64 MB |
| **vértice** | **32 B** (x,y,z, u,v, rgba, light, flags) |
| colisão por região / triângulo | 8³ shape, header 32 B, bloco 80 B / tri 40 B |
| `WALKABLE_NY` / `FLOOR_SAMPLES` | `0,7` / `13` |
| dano SKYRIM→MC | `÷ 5,0` |
| mixins | 37 classes, 51 pontos de injeção |
| comandos `REN_*` | 11 |
| mirror world | min_y −1024, altura 2048 |
| multiplayer | até 100 (LAN via e4mc) |

## GTA V

| Constante | Valor |
|---|---|
| host | ScriptHookV ASI + ReShade 6.8 |
| profundidade do GTA | **reversed-Z** |
| mapeamento | 1 metro = 1 bloco; (x,y,z) → (x, z+off, −y) |
| ângulos | MC yaw = 180 − heading; pitch = −pitch |
| objetos de script | máx ~400 (crasha ~1500) |

---

# APÊNDICE B — AS TRÊS FORMAS DE DAR PROFUNDIDADE

| | α overlay | β depth-mesh | γ shader | δ convidado desenha |
|---|---|---|---|---|
| **Tempo** | horas | ~2 sem | ~2 sem | ~1–2 sem |
| **Oclusão** | não | **sim** | **sim** | **sim** |
| **Luz do host no convidado** | não | sim (vértices) | **sim (shader)** | **sim (shader)** |
| **Reiluminação** | não | aproximada | **sim** | **sim** |
| **Depende do pipeline interno do host** | não | não | **sim** | **sim** |
| **Precisa da profundidade do host** | não | **sim** | **sim** | **não** |
| **Minecraft pode rodar devagar** | sim | sim | sim | **não importa** |
| **Reimplementado em** | ULTRAKILL | ULTRAKILL | GTA | SkyCraft |

⚠️ **A economia:** se você não tem acesso confiável à profundidade do host, β e γ estão fora. **δ não
precisa da profundidade** — só das matrizes, para colocar geometria no lugar certo. Então δ é o plano,
não o plano B.

---


## Os três passthroughs

| | Origem | Notas |
|---|---|---|
| **MinecraftInsideULTRAKILL 0.2.0** | release `0.2.0` (chavi) | `MinecraftPassthrough.dll` (BepInEx 5 + Harmony) + jar Fabric. Derivado do exemplo GTA. |
| **Minecraft dentro do GTA V** | `universal-modder` → `examples/minecraft-gta5-passthrough` | `gta/src/script.cpp`, `gta/shaders/MCPassthrough.fx`, `mc/`, `host/fakehost.py`, `gta/tests/fakegta.cpp`. Notas em `knowledge/games/gta-v/minecraft-passthrough.md` (**as 21 lições**). |
| **SkyCraft 0.1.0** | release `0.1.0` (chasmlol) | `SkyCraft.dll` (SKSE, CommonLibSSE-NG, DX11, Havok) + jar Fabric + Prism Launcher embutido. |

## A base de conhecimento

`github.com/rehan-remade/universal-modder` — 16 notas de jogos, 4 de técnicas, 12 playbooks de engine.

Mais relevantes:
- `knowledge/games/gta-v/minecraft-passthrough.md` — **as 21 lições.** As 1, 8, 9, 10, 20 custam mais.
- `knowledge/games/black-myth-wukong/reshade-depth-dead-end.md` — o **resultado negativo** de
  profundidade. Leia antes de escolher γ.
- `knowledge/techniques/oracles-how-agents-know-a-mod-works.md` — os 8 oráculos e o oráculo congelado.
- `knowledge/techniques/driving-real-games-safely.md` — nunca automatize clicks na tela de login.
- `skills/mashup-mods/SKILL.md` — o roteiro de mashup; este playbook cobre **apenas** o
  padrão de passthrough (dois processos vivos).
- `skills/mod-any-game/references/engines/*.md` — um por engine.
- `skills/mod-any-game/references/safety.md` — as regras e o porquê.
- `skills/mod-any-game/references/case-studies.md` — Terraria, AoE2, GTA, a onda de mashups.

## Ferramentas

| Need | |
|---|---|
| Toolkit CLI | `um scan <jogo>`, `um win shot/drive/record`, `um backup`, `um publish check`, `um kb search` |
| Decomp managed | `ilspycmd -p -o ~/<jogo>-decomp Assembly-CSharp.dll`, dnSpyEx |
| Decomp IL2CPP | Cpp2IL, Il2CppDumper (+ Ghidra/IDA nos corpos nativos) |
| Decomp Java | Vineflower, CFR, Procyon, Recaf |
| Decomp nativo | Ghidra (MCP: GhidraMCP, pyghidra-mcp), IDA (Hex-Rays MCP), ReVa |
| Gráficos | RenderDoc (+ renderdoc-mcp) |
| Live | Cheat Engine, x64dbg, Frida, UnityExplorer, UE4SS Live View, REFramework-MCP |
| ⚠️ | **Tente extrair strings primeiro.** O build do SkyCraft preservou RTTI e entregou a arquitetura inteira sem decompilador. |

## Minecraft

- Modrinth / Fabric — rede mais ativa do Minecraft
- Mixin + **MixinExtras** (`@WrapOperation` vem daqui). ⚠️ O SkyCraft usa `@WrapOperation` em 13
  mixins e **não declara MixinExtras em `depends`** — está transitivo. Se você copiar, declare.

## Licenças

| | |
|---|---|
| MIT | BepInEx, Harmony, Fabric, Fabric API, Java-WebSocket, CommonLibSSE-NG, spdlog, {fmt}, xbyak, SimpleIni, os três projetos de referência |
| BSD-3 | xbyak |
| Apache-2.0 | Fabric API |
| ⚠️ **GPL-3.0** | **Prism Launcher** — se você embute o launcher, o bundle fica sob GPL |
| Sem redistribuição | ScriptHookV, ReShade (o exemplo só automatiza o download) |

⚠️ **Disclosure:** os três projetos de referência declaram uso de IA. O ULTRAKILL é "written with Claude
Code and the universal-modder plugin, directed and play-tested by chavi". O GTA é "Written with Claude
Code", com inspiração no passthrough Minecraft-in-Skyrim do chasm e no Minecraft-in-Elden-Ring do
TobynJacobs. **Seja honesto sobre o que é IA** — comunidades reagem mal a releases "vibe-coded" não
declarados, e alguns Discords de recomp **banem** projetos de IA.

---

*Construído a partir de: `ilspycmd` sobre `MinecraftPassthrough.dll`; `vineflower` sobre o jar do
ULTRAKILL e o jar do SkyCraft; a nota de campo `portalcraft-minecraft-inside-portal-2.md`
(Preface: **câmera prevista + reprojeção near-to-far**, §0.6b, §2.15, §2.16, §5.8, §10.4, §11.5b/c); extração de strings + RTTI sobre `SkyCraft.dll` (3,6 MB, PE32+ x86-64,
MSVC RelWithDebInfo, 9 seções, RTTI preservado); a base de conhecimento `universal-modder`; e o
código-fonte do exemplo `minecraft-gta5-passthrough`.*

*⚠️ **O que NÃO foi verificado:** nada foi executado. Nenhuma das três engines estava instalada. ✅ =
leitura de código decompilado e de símbolos no binário. 📚 = conhecimento geral de engenharia. As
instruções de §2 são **contratos a preencher**, não código testado — o trabalho de adaptá-las ao jogo
concreto é seu e não está aqui.*
