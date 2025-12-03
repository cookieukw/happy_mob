# 🐾 Happy Mob

[![Minetest](https://img.shields.io/badge/Minetest-5.0+-blue?logo=minetest)](https://www.minetest.net/)  
[![Version](https://img.shields.io/badge/version-1.0.0-green)](#)  
[![License](https://img.shields.io/badge/license-MIT-yellow)](#)

**Autor:** CookieUkw  
**Versão:** 1.0.0  
**Data:** 28/02/2025  
**Dependências:** `default`, `mobs`

[!NOTE] É necessário instalar a dependência `mobs`, que vem junto do mod [Mobs Redo API](https://content.luanti.org/packages/TenPlus1/mobs/)

---

## 📖 Visão Geral

O **Happy Mob** é um mod para **Minetest** que adiciona uma criatura amigável e feliz ao seu mundo.  
Inspirado em pets virtuais, foi feito para interagir de forma positiva com jogadores, trazendo mais vida ao ambiente.

Ele vaga pelo mapa, mas fica animado quando alguém se aproxima, pulando de alegria. É passivo (não ataca), mas foge se for magoado.



---

## 🎮 Para Jogadores

### Funcionalidades

- **Comportamento autónomo:** anda em direções aleatórias explorando o ambiente.
- **Pulo de alegria:** ao detectar jogador a ≤ 3 blocos, pode pular verticalmente. O jogo mostra uma mensagem no chat.
- **Interação por clique:** clique direito faz o mob pular.
- **Mecanismo de fuga:** se atacado, corre na direção oposta por 10s.
- **Modelo 3D personalizado:** usa `.obj` para visual único.

### Instalação

1. **Descarregar o mod** → crie a pasta `happy_mob` com os arquivos.
2. **Mover para a pasta de mods:**
   - Windows: `minetest/mods/`
   - Linux: `~/.minetest/mods/`
   - macOS: `~/Library/Application Support/minetest/mods/`
3. **Ativar no jogo:** abra o Minetest → selecione o mundo → Configurar → ative **Happy Mob**.

### Como Encontrá-lo

- Spawna naturalmente em biomas com relva.
- No modo criativo, use o ovo de spawn **Happy Mob** para invocar instantaneamente.

---

## 🛠️ Para Desenvolvedores

### Estrutura dos Arquivos

- **`init.lua`** → lógica principal (comportamento, IA, callbacks).
- **`spawn.lua`** → regras de spawn (biomas, frequência, limites).
- **`mod.conf`** → metadados (nome, autor, dependências).
- **`happy_mob.obj`** → modelo 3D do mob.

### Análise do Código (init.lua)

#### 1. Configurações Iniciais

- `spawn_nodes`: blocos onde pode nascer (`default:stone`, etc.).
- `spawn_chance`: probabilidade de spawn.
- `mob_textures`: lista de texturas possíveis.
- `DEBUG`: ativa logs no console.

#### 2. Registro do Mob (`mobs:register_mob`)

- **Tipo/passividade:** `type = "animal"`, `passive = true`.
- **HP e física:** `hp_min`, `hp_max`, `collisionbox`, `stepheight`.
- **Visual:** `visual = "mesh"`, `mesh = "happy_mob.obj"`, `textures = {...}`.
- **Animações:** frames para `stand`, `walk`, `run`, `jump`.

#### 3. Máquina de Estados e Callbacks

- **Estados:**

  - `idle`: parado 3–8s.
  - `walking`: anda em direção/duração aleatória.
  - `jumping`: temporário, retorna a `idle`.
  - `fleeing`: ativado ao levar dano, foge por 10s.

- **Callbacks principais:**
  - `on_activate`: inicializa como `idle`.
  - `on_step`: IA frame a frame (pulos, fuga, proximidade de players).
  - `on_punch`: inicia fuga.
  - `on_rightclick`: faz pular.

#### 4. Spawn e Ovo Criativo

- `mobs:spawn()`: usa regras de `spawn.lua` e `init.lua`.
- `mobs:register_egg()`: adiciona ovo criativo do **Happy Mob**.

---
