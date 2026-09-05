# OrcaSlicer MCP — Claude conectado ao seu OrcaSlicer

## 🇧🇷 O que é isto?

### O problema que isso resolve

Quem imprime em 3D sabe: quando uma peça sai errada — suporte grudado demais, primeira camada mal aderida, peça frágil, *stringing*, acabamento ruim — o culpado quase sempre está escondido em algum parâmetro do slicer que a maioria das pessoas nunca abriu, nem sabe que existe. E mesmo quando você sabe *qual* configuração mexer, raramente sabe *para qual valor* mudar sem tentativa e erro.

Este projeto conecta o Claude diretamente aos arquivos de configuração do seu **OrcaSlicer** — não importa se você imprime em Bambu Lab, Prusa, Creality, Voron ou qualquer outra marca suportada. Na prática, isso transforma o Claude em um assistente técnico de impressão 3D que **realmente enxerga sua configuração de verdade**: seu perfil, seus valores, sua impressora, seu material — não um chute genérico.

### Como isso ajuda no dia a dia

**Se você não sabe onde mexer:** em vez de vasculhar dezenas de abas do programa procurando "aquela configuração de suporte", você só descreve o problema em português comum. O Claude traduz isso para a chave técnica certa, olha o valor atual no seu perfil, e explica o que está acontecendo.

**Se você é iniciante em impressão 3D:** não precisa saber o que é "Z-distance" ou "sparse infill pattern" de antemão. Você descreve o sintoma ("a peça quebrou fácil", "ficou uma coisa fiapenta saindo entre as partes", "o suporte não sai"), e o Claude relaciona isso com os parâmetros prováveis, explica o porquê, e sugere o ajuste.

**Fluxo típico:**
1. Você tira uma foto da impressão com problema (ou simplesmente descreve).
2. Você conta o que aconteceu: "o suporte ficou impossível de tirar", "a peça está mole", "quero mais resistência mas sem gastar muito mais material".
3. O Claude consulta seu perfil real (não um genérico) e identifica os parâmetros relacionados.
4. Ele propõe uma mudança concreta, mostrando o antes/depois — nada é alterado sem você ver e aprovar.
5. Se você aprovar, a mudança é aplicada com backup automático — dá pra desfazer a qualquer momento.

**Também serve para o caminho inverso:** se você já manja de impressão 3D e só quer economia de tempo, pode pedir comparações entre dois perfis, exportar/importar pacotes de configuração, ou ajustar parâmetros específicos diretamente, sem precisar navegar pela interface do programa.

### Sobre este projeto

Este é o motor de leitura/edição de presets do projeto irmão **[Anycubic Slicer Next MCP](#)** (link pro outro repositório), extraído e adaptado especificamente para o OrcaSlicer "puro" — já que o Anycubic Slicer Next é um fork direto do OrcaSlicer e usa exatamente o mesmo formato de arquivos. Todo o núcleo (leitura, resolução de herança `inherits`, comparação, edição segura com backup) é idêntico; só os caminhos padrão de instalação mudam.

**O que ainda não está incluído aqui** (mas existe no projeto irmão, e é portável pra cá): fatiamento real via linha de comando (`get_model_info`, `slice_model`, `compare_slice_configs`). Já validamos que o próprio OrcaSlicer aceita fatiamento headless real (`--slice`, `--load-settings`, `--export-3mf`) — testamos isso de ponta a ponta. Portar essas tools pra este pacote é um próximo passo natural, não um problema em aberto.

---

## 🇬🇧 What is this?

### The problem this solves

Anyone who prints in 3D knows the drill: when a print comes out wrong — support stuck too hard, poor first-layer adhesion, a part that's too fragile, stringing, rough finish — the culprit is almost always hiding in some slicer parameter most people have never opened, or don't even know exists. And even when you know *which* setting to touch, you rarely know *what value* to change it to without trial and error.

This project connects Claude directly to your **OrcaSlicer** configuration files — whether you print on Bambu Lab, Prusa, Creality, Voron, or any other supported brand. In practice, that turns Claude into a 3D-printing technical assistant that **actually sees your real configuration**: your profile, your values, your printer, your material — not a generic guess.

### How this helps day to day

**If you don't know where to look:** instead of digging through dozens of tabs hunting for "that one support setting," you just describe the problem in plain language. Claude translates that into the right technical key, checks the current value in your actual profile, and explains what's going on.

**If you're new to 3D printing:** you don't need to know what "Z-distance" or "sparse infill pattern" means beforehand. You describe the symptom ("the part broke too easily," "there's stringy stuff between the parts," "the support won't come off"), and Claude connects that to the likely parameters, explains why, and suggests the fix.

**Typical flow:**
1. You take a photo of the problem print (or just describe it).
2. You explain what happened: "the support was impossible to remove," "the part feels weak," "I want more strength without using much more material."
3. Claude checks your actual profile (not a generic one) and identifies the related parameters.
4. It proposes a concrete change, showing before/after — nothing changes without you seeing and approving it.
5. If you approve, the change is applied with an automatic backup — you can undo it at any time.

**It also works the other way:** if you already know 3D printing well and just want to save time, you can ask for comparisons between two profiles, export/import configuration bundles, or tweak specific parameters directly, without navigating the program's UI at all.

### About this project

This is the preset reading/editing engine from the sibling project **[Anycubic Slicer Next MCP](#)** (link to the other repo), extracted and adapted specifically for "vanilla" OrcaSlicer — since Anycubic Slicer Next is a direct fork of OrcaSlicer and uses exactly the same file format. The entire core (reading, `inherits` chain resolution, comparison, safe editing with backup) is identical; only the default installation paths differ.

**What isn't included here yet** (but exists in the sibling project, and is portable to this one): real slicing via command line (`get_model_info`, `slice_model`, `compare_slice_configs`). We've already validated that OrcaSlicer itself accepts real headless slicing (`--slice`, `--load-settings`, `--export-3mf`) — tested end to end. Porting these tools to this package is a natural next step, not an open problem.

---

# Documentação técnica / Technical documentation

## 🇬🇧 English

### What this package contains — `orcaslicer-presets-lite.mcpb`

A single-click Claude Desktop extension (MCPB format). Runs 100% locally, reads/writes only your OrcaSlicer preset files on disk. No login, no internet access, no cloud account needed.

**17 tools:**

| Tool                     | What it does                                                                                                   |
| ------------------------ | -------------------------------------------------------------------------------------------------------------- |
| `get_slicer_info`        | Reports OS and whether the configured preset folders exist                                                     |
| `list_printer_profiles`  | Lists all machine (printer) presets                                                                            |
| `list_filament_profiles` | Lists all filament presets                                                                                     |
| `list_process_profiles`  | Lists all process (print profile) presets                                                                      |
| `get_profile`            | Reads one preset exactly as stored on disk (raw, with its `inherits` chain unresolved)                         |
| `resolve_profile`        | Resolves the full `inherits` chain and returns the final effective values — what the slicer would actually use |
| `get_setting`            | Looks up a single setting's effective value in a preset                                                        |
| `get_settings`           | Looks up several related settings at once (e.g. all `support_*` keys)                                          |
| `compare_profiles`       | Diffs two presets of the same type and shows only what differs                                                 |
| `resolve_pt_name`        | Translates a Portuguese UI label (e.g. "Número de paredes") into its JSON key (`wall_loops`)                   |
| `propose_profile_patch`  | Calculates a proposed change and shows a before/after diff — writes nothing                                    |
| `validate_profile_patch` | Checks a proposed change against a semantic catalog (correct namespace, known limits)                          |
| `clone_profile`          | Creates a new user preset inheriting from an existing one                                                      |
| `apply_profile_patch`    | Writes a change to a user preset, with an automatic timestamped backup                                         |
| `rollback_profile`       | Restores the previous version of a preset from its last backup                                                 |
| `export_anycubic_bundle` | Packages presets into an `.orca_printer` bundle (zip) for re-import                                            |
| `import_anycubic_bundle` | Reads and validates an exported bundle without installing it                                                   |

Every write action follows a strict **READ → PROPOSE → (your confirmation) → APPLY** flow; nothing is changed on disk without you seeing a diff first, and every change can be rolled back.

### Installing

1. Download `orcaslicer-presets-lite.mcpb`.
2. Double-click it (or drag it onto the Claude Desktop window, or go to **Settings → Extensions → Advanced settings → Install Extension…**).
3. Claude Desktop shows an install screen asking for two folders:
   4. **System presets folder** — you'll need to pick this manually: browse to `/Applications/OrcaSlicer.app/Contents/Resources/profiles/<your printer brand>` (macOS) and choose the subfolder matching your printer's brand (e.g. `BBL`, `Prusa`, `Creality`, `Voron`, `Custom`) — **not** the whole `profiles` folder.
   5. **Your presets folder** — browse to `~/Library/Application Support/OrcaSlicer/user/<your numeric ID>/` (find your ID by opening that `user` folder in Finder — it's the only subfolder there).
4. Click **Install**.
5. Start a **new** conversation and try: *"List my process profiles"* or *"Resolve profile X and show me its support settings."*

No Python, no Homebrew, no terminal commands. Claude Desktop's built-in **UV runtime** downloads whatever it needs automatically the first time it runs (a few seconds).

### ⚠️ Honest caveat on default paths

The paths above are based on the known convention for macOS apps built on the same framework (wxWidgets) as OrcaSlicer — **they have not been confirmed against a real OrcaSlicer installation** (unlike the Anycubic Slicer Next version, which was tested against real user data). If your install uses different paths, just browse to the correct folders manually in the install screen — the tools work the same either way once the folders are right. If you try this and the default paths don't match, please open an issue with what you found — helps fix it for the next person.

**Windows** paths follow the same pattern with `%AppData%\OrcaSlicer\` instead of `~/Library/Application Support/OrcaSlicer/`.

### Coming next (not built yet, but validated as feasible)

Real headless slicing — `get_model_info`, `slice_model`, `compare_slice_configs` — letting you send an STL/3MF, describe a goal ("this is a keychain, more strength, PLA"), and get back real print-time and filament-usage numbers for 2-3 candidate configurations, not estimates. We already confirmed OrcaSlicer's command-line interface supports this (`--slice`, `--load-settings`, `--export-3mf`, `--info`) and tested it end-to-end. See the sibling [Anycubic Slicer Next MCP](#) repo for the working implementation — porting it here mostly means adjusting default paths.

## 🇧🇷 Português

### O que este pacote contém — `orcaslicer-presets-lite.mcpb`

Uma extensão de instalação em um clique para o Claude Desktop (formato MCPB). Roda 100% localmente, lê/grava só os arquivos de preset do OrcaSlicer no seu disco. Sem login, sem acesso à internet, sem conta na nuvem.

**17 tools:**

| Tool                     | O que faz                                                                                                  |
| ------------------------ | ---------------------------------------------------------------------------------------------------------- |
| `get_slicer_info`        | Informa o sistema operacional e se as pastas de preset configuradas existem                                |
| `list_printer_profiles`  | Lista todos os presets de máquina (impressora)                                                             |
| `list_filament_profiles` | Lista todos os presets de filamento                                                                        |
| `list_process_profiles`  | Lista todos os presets de processo (perfil de impressão)                                                   |
| `get_profile`            | Lê um preset exatamente como gravado em disco (bruto, sem resolver `inherits`)                             |
| `resolve_profile`        | Resolve toda a cadeia de `inherits` e retorna os valores efetivos finais — o que o slicer realmente usaria |
| `get_setting`            | Consulta o valor efetivo de uma única configuração em um preset                                            |
| `get_settings`           | Consulta várias configurações relacionadas de uma vez (ex.: todas as chaves `support_*`)                   |
| `compare_profiles`       | Compara dois presets do mesmo tipo e mostra só o que difere                                                |
| `resolve_pt_name`        | Traduz um nome da interface em português (ex.: "Número de paredes") para a chave JSON (`wall_loops`)       |
| `propose_profile_patch`  | Calcula uma mudança proposta e mostra o antes/depois — não grava nada                                      |
| `validate_profile_patch` | Verifica uma mudança proposta contra um catálogo semântico (namespace correto, limites conhecidos)         |
| `clone_profile`          | Cria um novo preset de usuário herdando de um existente                                                    |
| `apply_profile_patch`    | Grava uma mudança em um preset de usuário, com backup automático com timestamp                             |
| `rollback_profile`       | Restaura a versão anterior de um preset a partir do último backup                                          |
| `export_anycubic_bundle` | Empacota presets em um bundle `.orca_printer` (zip) para reimportar                                        |
| `import_anycubic_bundle` | Lê e valida um bundle exportado sem instalá-lo                                                             |

Toda ação de escrita segue rigorosamente o fluxo **READ → PROPOSE → (sua confirmação) → APPLY**; nada é alterado em disco sem você ver o diff antes, e toda mudança pode ser desfeita.

### Instalando

1. Baixe `orcaslicer-presets-lite.mcpb`.
2. Dê duplo clique nele (ou arraste para a janela do Claude Desktop, ou vá em **Configurações → Extensões → Configurações avançadas → Instalar extensão…**).
3. O Claude Desktop mostra uma tela de instalação pedindo duas pastas:
   4. **Pasta de presets do sistema** — você vai precisar escolher manualmente: navegue até `/Applications/OrcaSlicer.app/Contents/Resources/profiles/<marca da sua impressora>` (macOS) e escolha a subpasta da marca certa (ex.: `BBL`, `Prusa`, `Creality`, `Voron`, `Custom`) — **não** a pasta `profiles` inteira.
   5. **Pasta dos seus presets** — navegue até `~/Library/Application Support/OrcaSlicer/user/<seu ID numérico>/` (encontre seu ID abrindo essa pasta `user` no Finder — é a única subpasta lá dentro).
4. Clique em **Instalar**.
5. Comece uma conversa **nova** e teste: *"Liste meus process profiles"* ou *"Resolva o perfil X e me mostre as configurações de suporte."*

Sem Python, sem Homebrew, sem comandos de terminal. O runtime **UV** embutido no Claude Desktop baixa o que for necessário automaticamente na primeira execução (leva alguns segundos).

### ⚠️ Aviso honesto sobre os caminhos padrão

Os caminhos acima são baseados na convenção conhecida de apps macOS construídos sobre o mesmo framework (wxWidgets) do OrcaSlicer — **não foram confirmados contra uma instalação real do OrcaSlicer** (diferente da versão Anycubic Slicer Next, que foi testada com dados reais de usuário). Se sua instalação usa caminhos diferentes, é só navegar manualmente até as pastas certas na tela de instalação — as tools funcionam igual assim que as pastas estiverem certas. Se você testar isso e os caminhos padrão não baterem, por favor abra uma issue contando o que encontrou — ajuda a corrigir pra próxima pessoa.

Caminhos no **Windows** seguem o mesmo padrão, com `%AppData%\OrcaSlicer\` no lugar de `~/Library/Application Support/OrcaSlicer/`.

### Próximos passos (ainda não construído aqui, mas validado como viável)

Fatiamento real via linha de comando — `get_model_info`, `slice_model`, `compare_slice_configs` — permitindo mandar um STL/3MF, descrever um objetivo ("isso é um chaveiro, mais resistência, PLA"), e receber números reais de tempo de impressão e consumo de filamento pra 2-3 configurações candidatas, não estimativas. Já confirmamos que a interface de linha de comando do OrcaSlicer suporta isso (`--slice`, `--load-settings`, `--export-3mf`, `--info`) e testamos de ponta a ponta. Veja o repositório irmão [Anycubic Slicer Next MCP](#) pra implementação funcionando — portar pra cá é principalmente ajustar os caminhos padrão.

---

## Uploading this file to your own GitHub repo / Subindo esse arquivo pro seu próprio GitHub

*No command line needed — this is entirely through the browser.*
*Sem necessidade de linha de comando — isso é feito inteiramente pelo navegador.*

1. Go to **github.com** → click the **+** icon (top right) → **New repository**.
2. Give it a name (e.g. `orcaslicer-mcp`), choose Public or Private, click **Create repository**.
3. On the new repo's page, click **"uploading an existing file"** (or **Add file → Upload files**).
4. Drag in these two files: `orcaslicer-presets-lite.mcpb` and this `README.md`.
5. Scroll down, click **Commit changes**.

Done — anyone with the repo link can now download the package and read the install instructions.
