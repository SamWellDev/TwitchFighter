# Twitch Fighter Backend - Implementation Plan

Backend using C# ASP.NET Core 9 with PostgreSQL, SignalR, and Twitch integration.

## Architecture

```
┌─────────────────┐     SignalR       ┌─────────────────┐     EventSub     ┌──────────┐
│   Vue Frontend  │◄─────────────────►│  C# Backend     │◄───────────────►│  Twitch  │
│   (Port 3000)   │                   │  (Port 5041)    │                  │   API    │
└─────────────────┘                   └────────┬────────┘                  └──────────┘
                                               │
                                               ▼
                                      ┌─────────────────┐
                                      │   PostgreSQL    │
                                      └─────────────────┘
```

---

## ✅ Sistema Completo (2026-01-10)

Todos os componentes estão funcionando:
- OAuth flow corrigido (JSON snake_case → camelCase)
- RedirectUri corrigido para `http://localhost:5041/api/auth/callback`
- Login com Twitch funcionando ✅ **TESTADO**
- EventSub WebSocket conectado
- Broadcaster ID sendo definido automaticamente após login
- Frontend conectado ao backend via SignalR
- `twitchId` salvo em localStorage e passado ao `VisualizationArea`

---

## Progress Checklist

### ✅ Phase 1: Project Setup
- [x] Create ASP.NET Core project
- [x] Add NuGet packages (EF Core, TwitchLib, SignalR)
- [x] Configure PostgreSQL connection

### ✅ Phase 2: Database
- [x] User, Progress, Stats, Achievement models
- [x] AppDbContext with relationships
- [x] Initial migration applied
- [x] Achievement seeds

### ✅ Phase 3: SignalR
- [x] GameHub implementation
- [x] JoinGame/LeaveGame methods
- [x] CORS configured for Vue

### ✅ Phase 4: API Controllers
- [x] AuthController (OAuth login/callback)
- [x] UserController (profile endpoints)
- [x] ProgressController (wave, achievements)
- [x] StatsController (buff application)
- [x] RankingsController (leaderboards)

### ✅ Phase 5: Twitch EventSub
- [x] Validar Autenticação OAuth e API
- [x] TwitchEventSubService (IHostedService) - Build OK com TwitchLib v0.8.0
- [x] Integração AuthController → SetBroadcasterId → SubscribeToEvents
- [x] Scope `moderator:read:followers` adicionado
- [x] Testar com login Twitch real - ✅ Funcionando
- [x] JSON deserialization fix (snake_case → camelCase)
- [x] RedirectUri corrigido para backend (port 5041)

### ✅ Phase 6: SignalR Frontend Integration
- [x] Instalar `@microsoft/signalr` no frontend Vue
- [x] Criar composable `useSignalR()` em `src/composables/signalr.js`
- [x] Conectar ao GameHub (`/gamehub`) no mount do componente
- [x] Receber eventos `TwitchEvent` e aplicar buffs ao herói
- [x] Mostrar notificação visual quando buff é aplicado
- [x] Testar conexão end-to-end

### 🔄 Phase 7: Integração Frontend ↔ Backend

**Já existe no frontend (não precisa refazer):**
- ✅ Landing page com botão "Login with Twitch" (`LoginView.vue`)
- ✅ Dashboard com Config/Test tabs (`DashboardView.vue`)
- ✅ Botões de teste (Follow, Sub, Bits) - tab Test
- ✅ Preview da visualização no dashboard
- ✅ Toggle Live/Offline manual

**O que falta implementar:**
- [x] Conectar login ao backend (`/api/auth/login` ao invés de OAuth no frontend)
- [x] Receber `twitchId` do callback e armazenar em localStorage
- [x] Passar `twitchId` para `VisualizationArea` (ativar SignalR real)
- [x] Handler `@buffApplied` para aplicar buffs vindos do SignalR
- [x] Criar página `/overlay?id=xxx` para OBS (transparente, sem UI)
- [x] Implementar `exportOBS()` para copiar URL do overlay
- [ ] Detectar stream LIVE automaticamente via API Twitch

### ✅ Phase 8: Game Config System
- [x] Criar `gameconfig.json` com valores de buff e dificuldade
- [x] Criar `GameConfigService.cs` para ler config
- [x] Criar endpoint `GET /api/config`
- [x] Atualizar `TwitchEventSubService` para usar config
- [x] Instalar Axios no frontend
- [x] Criar `src/services/api.js`
- [x] Dashboard carrega config do backend na inicialização

### ✅ Phase 9: Test Mode Isolation
- [x] Separar stats de teste dos stats reais
- [x] Adicionar prop `testMode` ao VisualizationArea
- [x] Monstro "Dummy" imortal no modo teste (HP infinito)
- [x] Re-render completo ao trocar de tab (`:key="activeTab"`)
- [x] SignalR só conecta no modo Config
- [x] Visual normal no modo teste (não fica offline/grayscale)

### ✅ Phase 10: Game State Persistence
- [x] Carregar progresso do backend ao iniciar (wave, bestWave)
- [x] Carregar stats do backend ao iniciar (ATK, SPD, Crit)
- [x] Salvar progresso quando monstro morre (`recordDefeat`)
- [x] Persistir buffs recebidos via SignalR
- [x] Resetar estado no backend quando usuário clica Reset
- [x] Detectar stream LIVE automaticamente via API Twitch

### ✅ Phase 11: Dashboard/Overlay Architecture (COMPLETED 2026-01-12)
- [x] Identified double wave increment (frontend + backend)
- [x] Overlay is now MASTER - calls `recordDefeat`
- [x] Dashboard Config tab shows status panel (no game)
- [x] Dashboard Test tab has sandbox game with Dummy
- [x] Dashboard auto-syncs from backend every 10s
- [x] HP base now uses config (not hardcoded 50)
- [x] All texts in English

#### Architecture

| Component | Role |
|-----------|------|
| **Overlay** | Real game, calls `recordDefeat`, saves to backend |
| **Dashboard Config** | Status panel, auto-sync every 10s |
| **Dashboard Test** | Sandbox with immortal Dummy monster |

---

### 📋 Phase 12: Visual & Feature Improvements (TODO)
- [x] Remove blob fallback from monster sprite
- [ ] Generate new monster sprites (better quality)
- [ ] Add more hero skins
- [ ] Update monster HP values in `gameconfig.json`
- [ ] Add death/spawn animations
- [ ] Particle effects for critical hits
- [ ] Multiple monster sprites per wave
- [ ] Implement Global Rankings Panel

### ✅ Phase 13: Landing Page Polish & Demo Mode (2026-02-10)
- [x] Custom logo image (`/imgs/logo.png`) replacing text title
- [x] Custom card images (`/imgs/follow.png`, `/imgs/sub.png`, `/imgs/bits.png`)
- [x] Layout: logo at top, button + cards + footer at bottom (`justify-between`)
- [x] Background music player with play/pause and marquee song name
- [x] Footer with developer link and GitHub link (same as dashboard)
- [x] **Demo Mode** (`/dashboard?demo=true`): preview dashboard without Twitch account
  - Skips all API calls, SignalR, and stream polling
  - Goes directly to Test tab with Dummy monster
  - Config tab, Shop, Reset, and Export OBS buttons disabled
- [x] Proper logout function (clears `access_token`, `user_id`, `twitch_id`)

## Phase 6 Details: SignalR Frontend Integration

### Fluxo de Dados

```
┌──────────────────────────────────────────────────────────────────────────────────┐
│                              FLUXO DE EVENTOS TWITCH                             │
├──────────────────────────────────────────────────────────────────────────────────┤
│                                                                                  │
│   1. TWITCH                 2. BACKEND                    3. FRONTEND            │
│   ────────                  ───────────                   ──────────             │
│                                                                                  │
│   Viewer segue    ──────►   TwitchEventSubService        SignalR conectado       │
│   canal                     recebe evento                ao GameHub              │
│                                    │                            │                │
│                                    ▼                            │                │
│                             Calcula buff:                       │                │
│                             { Type: "atk",              ◄───────┘                │
│                               Value: 5 }                        │                │
│                                    │                            │                │
│                                    ▼                            │                │
│                             HubContext.SendAsync    ──────►  TwitchEvent         │
│                             ("TwitchEvent", ...)             callback            │
│                                                                  │               │
│                                                                  ▼               │
│                                                           heroStats.atk += 5    │
│                                                           Mostra notificação    │
│                                                                                  │
└──────────────────────────────────────────────────────────────────────────────────┘
```

### Arquivo: `src/composables/signalr.js` [NOVO]

Composable Vue 3 para gerenciar conexão SignalR:

```javascript
// Estrutura do composable
import * as signalR from "@microsoft/signalr";
import { ref, onUnmounted } from "vue";

export function useSignalR() {
  const connection = ref(null);
  const isConnected = ref(false);
  
  // Conectar ao hub e entrar no grupo do broadcaster
  async function connect(twitchId) { ... }
  
  // Desconectar
  async function disconnect() { ... }
  
  // Registrar callback para eventos Twitch
  function onTwitchEvent(callback) { ... }
  
  return { connection, isConnected, connect, disconnect, onTwitchEvent };
}
```

### Eventos Recebidos do Backend

O `TwitchEventSubService` envia eventos no seguinte formato:

| Evento | Payload |
|--------|---------|
| **Follow** | `{ Type: "follow", UserName: "viewer123", Buff: { Type: "atk", Value: 5 } }` |
| **Subscribe** | `{ Type: "subscription", UserName: "sub_user", Tier: "1000", Buff: { Type: "spd", Value: 10 } }` |
| **Cheer** | `{ Type: "bits", UserName: "cheerer", Bits: 100, Buff: { Type: "crit_chance", Value: 10 } }` |

### Modificações em `VisualizationArea.vue`

```javascript
// Importar e usar o composable
import { useSignalR } from '../composables/signalr';

const { connect, onTwitchEvent, isConnected } = useSignalR();

onMounted(async () => {
  // ... código existente ...
  
  // Conectar ao SignalR (usando ID do broadcaster logado)
  await connect("BROADCASTER_TWITCH_ID");
  
  // Registrar handler para eventos
  onTwitchEvent((event) => {
    applyBuff(event.Buff.Type, event.Buff.Value);
    showBuffNotification(event);
  });
});

// Aplicar buff aos stats do herói
function applyBuff(type, value) {
  switch(type) {
    case "atk": heroStats.atk += value; break;
    case "spd": heroStats.spd += value; break;
    case "crit_chance": heroStats.critChance += value; break;
  }
}
```

### Verificação

1. **Iniciar backend:** `cd TwitchFighter.API && dotnet run`
2. **Instalar deps:** `npm install`
3. **Iniciar frontend:** `npm run dev`
4. **Abrir browser:** `http://localhost:3000`
5. **Verificar console:** mensagens "SignalR connected" e "Joined game hub"
6. **Testar login:** `http://localhost:5041/api/auth/login`
7. **Verificar evento:** quando alguém seguir, o buff deve aparecer no jogo

---

## API Reference

### Authentication
| Endpoint | Description |
|----------|-------------|
| `GET /api/auth/login` | Redirect to Twitch |
| `GET /api/auth/callback` | Handle OAuth |
| `GET /api/auth/validate` | Validate token |

### User & Progress
| Endpoint | Description |
|----------|-------------|
| `GET /api/user/{id}` | User profile |
| `GET /api/progress/{userId}` | Wave progress |
| `POST /api/progress/{userId}/wave` | Update wave |
| `POST /api/progress/{userId}/defeat` | Record kill |

### Stats & Rankings
| Endpoint | Description |
|----------|-------------|
| `GET /api/stats/{userId}` | Current stats |
| `POST /api/stats/{userId}/buff` | Apply buff |
| `GET /api/rankings/global` | Leaderboard |
| `GET /api/rankings/rank/{userId}` | User rank |

---

## Running the Backend

```bash
cd TwitchFighter.API

# First time: apply migrations
dotnet ef database update

# Run the server
dotnet run
```

API available at: `http://localhost:5041`
SignalR Hub: `http://localhost:5041/gamehub`

---

## Configuration

Edit `appsettings.json` (adicionado ao .gitignore):

```json
{
  "ConnectionStrings": {
    "DefaultConnection": "Host=localhost;Database=twitchfighter;Username=postgres;Password=YOUR_PASSWORD"
  },
  "Twitch": {
    "ClientId": "YOUR_CLIENT_ID",
    "ClientSecret": "YOUR_SECRET_HERE",
    "RedirectUri": "http://localhost:5041/api/auth/callback"
  },
  "Cors": {
    "AllowedOrigins": ["http://localhost:3000"]
  }
}
```
