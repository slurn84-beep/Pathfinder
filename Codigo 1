<!DOCTYPE html>
<html lang="es">
<head>
<meta charset="utf-8" />
<meta name="viewport" content="width=device-width,initial-scale=1" />
<title>Ficha Pathfinder - Editor (Principal)</title>
<style>
  :root{
    --bg: #071020;
    --panel: rgba(255,255,255,0.04);
    --glass: rgba(255,255,255,0.03);
    --glass-strong: rgba(255,255,255,0.05);
    --gold: #ffd86b;
    --muted: #b6c0cc;
    --accent: rgba(0,0,0,0.35);
    --blue-glass: rgba(20,40,70,0.35);
    --gold-glass: rgba(90,60,10,0.22);
    --green-glass: rgba(10,60,40,0.28);
    --orange-glass: rgba(90,45,5,0.25);
    --glass-border: rgba(255,255,255,0.06);
    --card-radius: 12px;
    --glass-blur: 6px;
    --text: #e9eef6;
  }

  *{box-sizing:border-box}
  body{
    margin:0; min-height:100vh; font-family:Inter, "Segoe UI", Roboto, Arial, sans-serif;
    background: linear-gradient(135deg,#071020 0%, #08162a 100%);
    color:var(--text); -webkit-font-smoothing:antialiased; -moz-osx-font-smoothing:grayscale;
  }

  .app { max-width:1300px; margin:18px auto; display:grid; grid-template-columns: 320px 1fr; gap:18px; padding:18px; }

  /* SIDEBAR */
  aside.sidebar{
    background: linear-gradient(180deg, rgba(255,255,255,0.02), rgba(255,255,255,0.01));
    border:1px solid rgba(255,215,0,0.06);
    border-radius:var(--card-radius); padding:14px; position:relative;
    backdrop-filter: blur(6px);
  }
  .avatar-wrap{ display:flex; gap:12px; align-items:center; }
  .avatar {
    width:96px; height:96px; border-radius:50%; overflow:hidden; display:flex; align-items:center; justify-content:center;
    background: linear-gradient(180deg,#071018,#0f2133); border:4px solid rgba(255,215,0,0.08); position:relative;
  }
  .avatar img{ width:100%; height:100%; object-fit:cover; display:block }
  .avatar-svg{ position:absolute; inset:0; pointer-events:none; }
  .char-headline { font-size:1.05rem; font-weight:700; color:var(--gold); }
  .small { font-size:0.82rem; color:var(--muted); }

  .block { margin-top:14px; padding-top:10px; border-top:1px dashed rgba(255,255,255,0.03); }
  .block h3{ margin:0 0 10px 0; color:var(--gold); font-size:0.95rem; display:flex; justify-content:space-between; align-items:center; }
  .data-grid { display:grid; gap:8px; }

  select,input[type="number"],input[type="text"]{
    width:100%; padding:8px 10px; border-radius:8px; background:var(--glass); border:1px solid var(--glass-border); color:var(--text);
  }

  .sidebar-footer{ display:flex; gap:8px; margin-top:12px; }
  .btn { padding:8px 10px; border-radius:8px; cursor:pointer; border:none; background:rgba(255,255,255,0.03); color:var(--text); }
  .btn.primary{ background:linear-gradient(90deg,var(--gold), #ffea6a); color:#081223; font-weight:700; }

  /* MAIN */
  .main { background:var(--accent); border-radius:var(--card-radius); padding:14px; border:1px solid rgba(255,255,255,0.03); min-height:520px; backdrop-filter: blur(6px); }
  .tabs { display:flex; gap:8px; border-bottom:1px solid rgba(255,255,255,0.03); padding-bottom:8px; margin-bottom:12px; }
  .tab-btn { background:transparent; color:var(--muted); padding:8px 12px; border-radius:8px; border:none; cursor:pointer; }
  .tab-btn.active{ background: linear-gradient(90deg, rgba(255,215,0,0.07), rgba(255,215,0,0.02)); color:var(--gold); font-weight:700; }

  .tab-content{ display:none; }
  .tab-content.active{ display:block; }

  /* layout principal: full width block, then two columns (attributes & saves), then xp full width */
  .full-width{ width:100%; }
  .card{ background:linear-gradient(180deg, rgba(255,255,255,0.02), rgba(0,0,0,0.25)); padding:12px; border-radius:10px; border:1px solid rgba(255,255,255,0.03); }
  .panel-row { display:block; }

  .top-basic { margin-bottom:12px; }
  .basic-grid{ display:flex; gap:12px; flex-wrap:wrap; align-items:flex-start; }

  /* specifics: colored cards inside */
  .card.basic { background: linear-gradient(180deg,var(--blue-glass), rgba(10,20,40,0.25)); border:1px solid rgba(25,90,160,0.08); box-shadow: 0 6px 18px rgba(2,6,12,0.35); }
  .card.attrs  { background: linear-gradient(180deg,var(--gold-glass), rgba(40,30,10,0.12)); border:1px solid rgba(200,150,40,0.06); }
  .card.saves  { background: linear-gradient(180deg,var(--green-glass), rgba(6,30,20,0.12)); border:1px solid rgba(40,180,120,0.06); }
  .card.xp     { background: linear-gradient(180deg,var(--orange-glass), rgba(80,40,10,0.10)); border:1px solid rgba(220,150,80,0.06); }

  /* attributes vertical */
  .attributes { display:flex; gap:12px; }
  .attr-col{ display:flex; flex-direction:column; gap:8px; min-width:260px; }
  .attr-row{ display:flex; justify-content:space-between; gap:8px; align-items:center; padding:8px; border-radius:8px; background: rgba(0,0,0,0.12); }
  .attr-name{ font-weight:700; color:var(--gold); width:80px; }
  .attr-values{ display:flex; gap:8px; align-items:center; flex:1; justify-content:flex-end; }
  .attr-values input{ width:74px; }

  /* saves layout (table-like compact) */
  .saves-grid{ display:grid; grid-template-columns: 1fr; }
  .saves-header{ display:flex; gap:8px; align-items:center; margin-bottom:8px; font-weight:700; color:var(--muted); padding:6px 8px; }
  .saves-header div{ flex:1; text-align:center; font-size:0.85rem; }
  .save-row{ display:flex; gap:8px; align-items:center; padding:8px; border-radius:8px; background: rgba(0,0,0,0.10); margin-bottom:8px; }
  .save-row .save-label{ width:110px; font-weight:700; color:var(--text); }
  .save-row input{ width:86px; padding:6px; border-radius:8px; }

  /* experience small grid */
  .xp-grid{ display:grid; grid-template-columns: repeat(3,1fr); gap:8px; }
  .xp-cell{ padding:10px; border-radius:8px; background: rgba(0,0,0,0.12); display:flex; flex-direction:column; gap:6px; align-items:flex-start; }
  .xp-cell .label{ font-size:0.82rem; color:var(--muted); }
  .xp-cell .value{ font-weight:700; color:var(--gold); }

  /* responsive */
  @media (max-width:1000px){
    .app{ grid-template-columns: 1fr; padding:12px; }
    .attr-col{ min-width:unset; }
    .save-row{ flex-wrap:wrap; }
  }

  /* small helpers */
  .muted { color:var(--muted); font-size:0.85rem; }
  .right{ text-align:right; }
  label.small{ display:block; font-size:0.78rem; color:var(--muted); margin-bottom:6px; font-weight:700; }
</style>
</head>
<body>
<div class="app">
  <!-- SIDEBAR -->
  <aside class="sidebar" id="sidebar">
    <div class="avatar-wrap">
      <label class="avatar" title="Click para subir imagen">
        <svg class="avatar-svg" viewBox="0 0 100 100" preserveAspectRatio="none">
          <defs>
            <linearGradient id="g1" x1="0" x2="1"><stop offset="0" stop-color="#ffd700" stop-opacity="0.7"/><stop offset="1" stop-color="#ffd700" stop-opacity="0.15"/></linearGradient>
          </defs>
          <circle cx="50" cy="50" r="46" stroke="rgba(255,255,255,0.06)" stroke-width="2" fill="none"/>
          <circle id="progressCircle" cx="50" cy="50" r="44" stroke="url(#g1)" stroke-width="4" fill="none" stroke-dasharray="276.46" stroke-dashoffset="276.46" transform="rotate(-90 50 50)"/>
        </svg>
        <span id="avatarLabel">🧙‍♂️</span>
        <img id="avatarImg" src="" alt="avatar" style="display:none"/>
        <input id="avatarInput" type="file" accept="image/*">
      </label>
      <div style="flex:1">
        <div class="char-headline" id="charName">Nuevo Personaje</div>
        <div class="small">Jugador: <span id="playerNameDisplay">—</span></div>
        <div class="small">Clases: <span id="charClasses">—</span></div>
        <div class="small">Prestigios: <span id="charPrestige">—</span></div>
      </div>
    </div>

    <div class="block">
      <h3>📋 Clases</h3>
      <div class="data-grid">
        <select id="class1"></select>
        <select id="class2"></select>
        <select id="class3"></select>
        <div style="display:flex; gap:8px;">
          <input id="class1-level" type="number" min="0" value="1" placeholder="Nv1" style="width:33%">
          <input id="class2-level" type="number" min="0" value="0" placeholder="Nv2" style="width:33%">
          <input id="class3-level" type="number" min="0" value="0" placeholder="Nv3" style="width:33%">
        </div>
      </div>
    </div>

    <div class="block">
      <h3>🏅 Prestigios</h3>
      <div class="data-grid">
        <select id="prestige1"></select>
        <select id="prestige2"></select>
        <select id="prestige3"></select>
        <div style="display:flex; gap:8px;">
          <input id="prestige1-level" type="number" min="0" value="0" style="width:33%">
          <input id="prestige2-level" type="number" min="0" value="0" style="width:33%">
          <input id="prestige3-level" type="number" min="0" value="0" style="width:33%">
        </div>
      </div>
    </div>

    <div class="block">
      <h3>📜 Trasfondo</h3>
      <div class="data-grid">
        <select id="backgroundSelect"></select>
      </div>
    </div>

    <div class="block" style="display:flex; gap:8px;">
      <input id="playerName" type="text" placeholder="Nombre del jugador" style="flex:1;">
      <button class="btn" id="applyPlayer">OK</button>
    </div>

    <div class="sidebar-footer">
      <button class="btn" id="saveBtn">💾 Guardar</button>
      <button class="btn primary" id="exportBtn">📤 Exportar</button>
    </div>
  </aside>

  <!-- MAIN -->
  <main class="main">
    <div class="tabs">
      <button class="tab-btn active" data-tab="tab-basic">Principal</button>
      <button class="tab-btn" data-tab="tab-classes">Clases</button>
      <button class="tab-btn" data-tab="tab-prestige">Prestigio</button>
      <button class="tab-btn" data-tab="tab-combat">Combate</button>
      <button class="tab-btn" data-tab="tab-feats">Dotes & Habilidades</button>
      <button class="tab-btn" data-tab="tab-equip">Equipo</button>
      <button class="tab-btn" data-tab="tab-familiar">Familiar / Compañero</button>
      <button class="tab-btn" data-tab="tab-magic">Magia</button>
    </div>

    <!-- TAB: Principal -->
    <div id="tab-basic" class="tab-content active">
      <div class="panel-row">
        <div class="card basic top-basic full-width">
          <h3 style="color:var(--gold)">📋 Datos Básicos</h3>
          <div class="basic-grid" style="margin-top:10px;">
            <div style="flex:1; min-width:300px;">
              <label class="small">Nombre del Personaje</label>
              <input id="characterName" type="text" placeholder="Nombre personaje">
            </div>
            <div style="width:220px">
              <label class="small">Raza</label>
              <select id="raceSelect"></select>
            </div>
            <div style="width:160px">
              <label class="small">Alineamiento</label>
              <select id="alignment"></select>
            </div>
            <div style="width:220px">
              <label class="small">Deidad</label>
              <select id="deitySelect"></select>
            </div>

            <div style="width:120px">
              <label class="small">Altura (cm)</label>
              <input id="characterHeight" type="number" value="175" min="0">
            </div>
            <div style="width:140px">
              <label class="small">Tamaño</label>
              <input id="characterSize" type="text" readonly value="Mediano">
            </div>
            <div style="width:120px">
              <label class="small">Sexo</label>
              <select id="sex"><option>Masculino</option><option>Femenino</option><option>Otro</option></select>
            </div>
            <div style="width:120px">
              <label class="small">Edad</label>
              <input id="age" type="number" placeholder="Edad">
            </div>
          </div>
        </div>

        <div style="display:flex; gap:12px; margin-top:12px;">
          <!-- ATTRIBUTES (left) -->
          <div class="card attrs" style="flex:1;">
            <h3 style="color:var(--gold)">🔢 Atributos</h3>
            <div style="margin-top:8px;" class="attributes">
              <div class="attr-col" id="attrColumn">
                <!-- attribute rows created by JS -->
              </div>

              <div style="flex:1">
                <div class="small">Nota: Columnas → Base | Ajustes (raza/trasfondo/nivel) | Modificador</div>
                <div style="height:14px"></div>
                <div class="card" style="margin-top:12px;padding:8px;background:rgba(0,0,0,0.12)">
                  <div class="small">Puntos disponibles (DM)</div>
                  <div style="font-weight:700;color:var(--gold)" id="points-available">200</div>
                </div>
              </div>
            </div>
          </div>

          <!-- SAVES (right) -->
          <aside style="width:420px;">
            <div class="card saves">
              <h3 style="color:var(--gold)">🛡️ Salvaciones</h3>
              <p class="small" id="saves-note">(Los encabezados son globales; las casillas Base Clases/Base Prestigio son editables o se llenarán automáticamente cuando se agreguen tablas por clase.)</p>

              <div style="margin-top:8px;" class="saves-grid">
                <div class="saves-header">
                  <div>Concepto</div>
                  <div>Base clases</div>
                  <div>Base prestigio</div>
                  <div>Mod. Carac.</div>
                  <div>Mod. Varios</div>
                  <div>Total</div>
                </div>

                <!-- Fortaleza -->
                <div class="save-row">
                  <div class="save-label">Fortaleza</div>
                  <input id="save-fort-base" type="number" value="0" title="Base Clases">
                  <input id="save-fort-prestige" type="number" value="0" title="Base Prestigio">
                  <input id="save-fort-char" type="number" value="0" readonly title="Mod por CON">
                  <input id="save-fort-mod" type="number" value="0" title="Mods varios">
                  <div style="flex:1;text-align:right;font-weight:700;color:var(--gold)" id="save-fort-total">0</div>
                </div>

                <!-- Reflejos -->
                <div class="save-row">
                  <div class="save-label">Reflejos</div>
                  <input id="save-ref-base" type="number" value="0">
                  <input id="save-ref-prestige" type="number" value="0">
                  <input id="save-ref-char" type="number" value="0" readonly>
                  <input id="save-ref-mod" type="number" value="0">
                  <div style="flex:1;text-align:right;font-weight:700;color:var(--gold)" id="save-ref-total">0</div>
                </div>

                <!-- Voluntad -->
                <div class="save-row">
                  <div class="save-label">Voluntad</div>
                  <input id="save-will-base" type="number" value="0">
                  <input id="save-will-prestige" type="number" value="0">
                  <input id="save-will-char" type="number" value="0" readonly>
                  <input id="save-will-mod" type="number" value="0">
                  <div style="flex:1;text-align:right;font-weight:700;color:var(--gold)" id="save-will-total">0</div>
                </div>

              </div>
            </div>
          </aside>
        </div>

        <!-- XP full width -->
        <div style="margin-top:12px;" class="card xp full-width">
          <h3 style="color:var(--gold)">🎯 Experiencia</h3>
          <div style="margin-top:8px;" class="small">Las 3 casillas superiores muestran XP requerida por la(s) clases seleccionadas; las 3 inferiores por prestigios. El total sugerido para el siguiente nivel es la suma.</div>
          <div style="margin-top:10px; display:flex; gap:12px; align-items:flex-start;">
            <div style="flex:1">
              <div style="display:grid; grid-template-columns: repeat(3, 1fr); gap:8px; margin-top:8px;">
                <div class="xp-cell"><div class="label">Clase 1</div><div class="value" id="xp-class-1">—</div></div>
                <div class="xp-cell"><div class="label">Clase 2</div><div class="value" id="xp-class-2">—</div></div>
                <div class="xp-cell"><div class="label">Clase 3</div><div class="value" id="xp-class-3">—</div></div>

                <div class="xp-cell"><div class="label">Prest. 1</div><div class="value" id="xp-pre-1">—</div></div>
                <div class="xp-cell"><div class="label">Prest. 2</div><div class="value" id="xp-pre-2">—</div></div>
                <div class="xp-cell"><div class="label">Prest. 3</div><div class="value" id="xp-pre-3">—</div></div>
              </div>
            </div>

            <div style="width:240px;">
              <div class="card" style="padding:10px;">
                <div class="small">Total requerido próximo nivel (suma propuesta)</div>
                <div style="font-weight:700;color:var(--gold);font-size:1.1rem" id="xp-next">—</div>
                <div style="height:8px"></div>
                <div class="small">XP acumulada</div>
                <div style="font-weight:700;color:#fff" id="xp-total">0</div>
                <div style="height:8px"></div>
                <div style="display:flex; gap:8px;">
                  <input id="xp-session" type="number" value="0" style="flex:1; padding:8px; border-radius:8px;">
                  <button class="btn" id="add-xp">➕ Añadir</button>
                </div>
                <div style="height:8px"></div>
                <div class="small" id="xp-log"></div>
              </div>
            </div>
          </div>
        </div>

      </div>
    </div>

    <!-- Placeholder tabs (structure ready) -->
    <div id="tab-classes" class="tab-content">
      <div class="card"><h3 style="color:var(--gold)">📚 Clases</h3>
        <div class="small">Cada clase tendrá subpestañas con descripción, progresión por nivel (HP, BAB, salvaciones) y habilidades. Aquí se cargarán automáticamente cuando se provean las tablas completas.</div>
        <div id="classes-detail" style="margin-top:12px"></div>
      </div>
    </div>

    <div id="tab-prestige" class="tab-content">
      <div class="card"><h3 style="color:var(--gold)">🏅 Prestigios</h3>
        <div class="small">Detalle y progresión de prestigios (por ahora muestra lista seleccionada)</div>
        <div id="prestige-detail" style="margin-top:12px"></div>
      </div>
    </div>

    <div id="tab-combat" class="tab-content">
      <div class="card"><h3 style="color:var(--gold)">⚔️ Combate</h3>
        <div class="small">Lugar para CA, Iniciativa, Velocidad, Puntos de golpe por clase y total, armaduras y armas equipadas. (Se actualizará con las tablas por clase.)</div>
        <div id="combat-box" style="margin-top:10px;"></div>
      </div>
    </div>

    <div id="tab-feats" class="tab-content">
      <div class="card"><h3 style="color:var(--gold)">📜 Dotes & Habilidades</h3>
        <div class="small">Listado y asignación de dotes (activadas por nivel efectivo) y hoja de habilidades (por columnas). Implementación completa vendrá cuando se integre la lista de dotes con prerrequisitos en formato estructurado.</div>
      </div>
    </div>

    <div id="tab-equip" class="tab-content">
      <div class="card"><h3 style="color:var(--gold)">🎒 Equipo</h3>
        <div class="small">Inventario, monedas, objetos mágicos, etc. (Plantilla lista para poblar.)</div>
      </div>
    </div>

    <div id="tab-familiar" class="tab-content">
      <div class="card"><h3 style="color:var(--gold)">🐾 Familiar / Compañero</h3>
        <div class="small">Ficha reducida para compañeros/animal & humanoide (puede enlazar fichas completas externas).</div>
      </div>
    </div>

    <div id="tab-magic" class="tab-content">
      <div class="card"><h3 style="color:var(--gold)">✨ Magia</h3>
        <div class="small">Gestión de conjuros por clase (nivel, espacios, preparadas/conocidas). Pendiente integración por clase conjuradora.</div>
      </div>
    </div>

  </main>
</div>

<script>
/* =========================
   Datos iniciales (tomados / normalizados del chat)
   - RACES con algunos modificadores
   - CLASSES y PRESTIGE_LIST nombre + saves (básicos)
   - DEITIES (lista parcial que me diste)
   - BACKGROUNDS (lista pegada)
   - XP thresholds (N1..N20) según tabla que nos diste (N2=3000 etc.)
==========================*/

const RACES = {
  "Humano": { modifiers: {}, size: "Mediano" },
  "Branquial": { modifiers: { CON: 2, CHA: 2, WIS: -2 }, size: "Mediano" },
  "Dhampir": { modifiers: { DEX:2, CHA:2, CON:-2 }, size:"Mediano" },
  "Drow Plebeyo": { modifiers: { DEX:2, CHA:2, CON:-2 }, size:"Mediano" },
  "Drow Noble": { modifiers: { DEX:4, INT:2, WIS:2, CHA:2, CON:-2 }, size:"Mediano" },
  "Efrit": { modifiers: { DEX:2, CHA:2, WIS:-2 }, size:"Mediano" },
  "Duergar": { modifiers: { CON:2, WIS:2, CHA:-4 }, size:"Mediano" },
  "Estrícido": { modifiers: { DEX:2, CHA:-2 }, size:"Mediano" },
  "Félido": { modifiers: { DEX:2, WIS:-2, CHA:2 }, size:"Mediano" },
  "Svirfneblin": { modifiers: { STR:-2, DEX:2, WIS:2, CHA:-4 }, size:"Pequeño" },
  "Mediano": { modifiers: { DEX:2, CHA:2, STR:-2 }, size:"Pequeño" },
  "Orco": { modifiers: { STR:4, INT:-2, WIS:-2, CHA:-2 }, size:"Mediano" },
  "Kitsune": { modifiers: { DEX:2, CHA:2, STR:-2 }, size:"Mediano" },
  "Rátido": { modifiers: { STR:-2, DEX:2, INT:2 }, size:"Pequeño" },
  "Replicante": { modifiers: { WIS:2, CHA:2, CON:-2 }, size:"Mediano" },
  "Gnomo": { modifiers: { CON:2, CHA:2, STR:-2 }, size:"Pequeño" },
  "Semielfo": { modifiers: {}, size:"Mediano" },
  "Semiorco": { modifiers: {}, size:"Mediano" },
  "Suli": { modifiers: { STR:2, CHA:2, INT:-2 }, size:"Mediano" },
  "Tiflin": { modifiers: { DEX:2, INT:2, CHA:-2 }, size:"Mediano" },
  "Vanara": { modifiers: { DEX:2, WIS:2, CHA:-2 }, size:"Mediano" },
  "Vishkanya": { modifiers: { DEX:2, CHA:2, WIS:-2 }, size:"Mediano" }
};

const CLASSES = [
 "Adalid","Alquimista","Antipaladín","Arcanista","Bárbaro","Bardo","Bruja","Cazador","Chamán",
 "Clérigo","Convocador","Druida","Escaldo","Espadachín","Explorador","Exterminador","Guerrero",
 "Hechicero","Inquisidor","Investigador","Mago","Magus","Monje","Ninja","Oráculo","Paladín",
 "Pícaro","Pistolero","Psiónico","Pugilista","Rabioso de sangre","Sacerdote de guerra","Samurai"
];

const PRESTIGE_LIST = [
 {name:"Arquero Arcano", saves:{fort:1,ref:1,will:0}},
 {name:"Asesino", saves:{fort:0,ref:0,will:1}},
 {name:"Bribón Arcano", saves:{fort:0,ref:0,will:1}},
 {name:"Caballero Arcano", saves:{fort:1,ref:1,will:0}},
 {name:"Caminante del horizonte", saves:{fort:1,ref:1,will:0}},
 {name:"Cronista Pathfinder", saves:{fort:0,ref:0,will:1}},
 {name:"Danzarín Sombrío", saves:{fort:0,ref:0,will:1}},
 {name:"Defensor firme", saves:{fort:1,ref:1,will:1}},
 {name:"Discípulo del Dragón", saves:{fort:0,ref:1,will:0}},
 {name:"Duelista", saves:{fort:1,ref:0,will:1}},
 {name:"Espía maestro", saves:{fort:0,ref:0,will:1}},
 {name:"Guardián de la naturaleza", saves:{fort:0,ref:1,will:0}},
 {name:"Heraldo de batalla", saves:{fort:1,ref:1,will:1}},
 {name:"Maestro del Saber", saves:{fort:0,ref:0,will:1}},
 {name:"Profeta de la furia", saves:{fort:0,ref:1,will:0}},
 {name:"Químico maestro", saves:{fort:1,ref:1,will:1}},
 {name:"Teúrgo Místico", saves:{fort:0,ref:1,will:0}},
 {name:"Vindicador Sagrado", saves:{fort:1,ref:1,will:0}}
];

// Deidades (lista parcial y el alineamiento asociado donde se conoce)
const DEITIES = [
 "Azut (LN)","Beshaba (CM)","Chauntea (NB)","Eldat (NB)","Gáragos (CN)","Gárgozh (LM)",
 "Gond (N)","Grumbar (N)","Güeron Viéntrom (NB)","Hallador Puadeldraco (CN)","Hoar (LN)",
 "Ilmáter (LB)","Milil (NB)","Mystra (NB)","Perdición (LM)","Selûne (CB)","Sharess (CB)",
 "Shóndakul (CN)","Siamorfhe (LN)","Talos (CM)","Tempus (CN)","Torm (LB)","Tyr (LB)",
 "Ubtao (N)","Ulutiu (LN)","Úmberli (CM)","Úzhgar (CN)","Valkur (CB)","Velsharún (NM)","Waukin (N)"
];

// Backgrounds (lista que pegaste)
const BACKGROUNDS = [
 "Acólito","Artesano","Charlatán","Criminal","Animador","Campesino","Guardia","Guía",
 "Ermitaño","Comerciante","Noble","Erudito","Marinero","Escribano","Soldado","Vagabundo"
];

// XP thresholds by level (N1..N20). Using numbers you provided earlier for N2..N20; set N1=0
const XP_THRESHOLDS = {
  1:0, 2:3000, 3:7500, 4:14000, 5:23000, 6:35000, 7:53000, 8:77000, 9:115000, 10:160000,
  11:235000, 12:330000, 13:475000, 14:665000, 15:955000, 16:1350000, 17:2000000, 18:2700000,
  19:3850000, 20:5350000
};

/* =========================
   Estado inicial
==========================*/
const state = {
  name: "Nuevo Personaje",
  player: "",
  race: "Humano",
  alignment: "N",
  deity: "",
  height: 175,
  size: "Mediano",
  sex: "Masculino",
  age: 0,
  classes: [{name:"Guerrero", level:1},{name:null, level:0},{name:null, level:0}],
  prestiges: [{name:null, level:0},{name:null, level:0},{name:null, level:0}],
  background: "Acólito",
  xpTotal: 0,
  xpSession: 0,
  pointsAvailable: 200
};

/* =========================
   DOM helpers
==========================*/
function $ (id){ return document.getElementById(id); }
function createOption(text,val){ const o=document.createElement('option'); o.value = val||text; o.textContent = text; return o; }

/* =========================
   Inicialización UI
==========================*/
function populateSelects(){
  // races
  const raceSel = $('raceSelect'); raceSel.innerHTML = '';
  Object.keys(RACES).sort().forEach(r => raceSel.appendChild(createOption(r)));

  // deities
  const dsel = $('deitySelect'); dsel.innerHTML = '<option value="">—</option>';
  DEITIES.forEach(d=> dsel.appendChild(createOption(d)));

  // alignment (9)
  const aligns = ['LB','LN','LB?','CB','CN','CM','NB','N','NM','LM']; // but provide common 9
  const alignList = ['LB','LN','N','NB','CB','CN','CM','LM','NM'];
  const aSel = $('alignment'); aSel.innerHTML='';
  alignList.forEach(a=> aSel.appendChild(createOption(a)));

  // classes & prestige selects
  ['class1','class2','class3'].forEach(id=>{
    const sel = $(id); sel.innerHTML = '<option value="">—</option>';
    CLASSES.forEach(c => sel.appendChild(createOption(c)));
  });
  ['prestige1','prestige2','prestige3'].forEach(id=>{
    const sel = $(id); sel.innerHTML = '<option value="">—</option>';
    PRESTIGE_LIST.forEach(p => sel.appendChild(createOption(p.name)));
  });

  // backgrounds
  const bg = $('backgroundSelect'); bg.innerHTML = '';
  BACKGROUNDS.forEach(b => bg.appendChild(createOption(b)));

  // initial values
  $('raceSelect').value = state.race;
  $('alignment').value = state.alignment;
  $('deitySelect').value = state.deity;
  $('backgroundSelect').value = state.background;
  $('characterName').value = state.name;
}

/* =========================
   Attributes UI build
==========================*/
function buildAttributesUI(){
  const container = $('attrColumn'); container.innerHTML = '';
  const attributes = ['FUE','DES','CON','INT','SAB','CAR'];
  attributes.forEach(attr=>{
    const row = document.createElement('div'); row.className='attr-row';
    row.innerHTML = `
      <div style="display:flex;flex-direction:column">
        <div class="attr-name">${attr}</div>
        <div class="muted" style="font-size:0.75rem">Base / Ajuste / Mod</div>
      </div>
      <div class="attr-values">
        <input data-attr="${attr}" class="attr-base" type="number" value="${Math.floor(state.pointsAvailable/6)}">
        <input data-attr="${attr}" class="attr-adjust" type="number" value="0" readonly>
        <div style="width:48px;text-align:right;font-weight:700;color:var(--gold)" class="attr-mod" id="mod-${attr}">+0</div>
      </div>
    `;
    container.appendChild(row);
  });
}

/* compute ability mod */
function abilityModifier(score){ return Math.floor((score - 10)/2); }

/* update attribute mods from inputs */
function updateAttributeMods(){
  document.querySelectorAll('.attr-row').forEach(row=>{
    const base = parseInt(row.querySelector('.attr-base').value)||0;
    const adj  = parseInt(row.querySelector('.attr-adjust').value)||0;
    const total = base + adj;
    const mod = abilityModifier(total);
    row.querySelector('.attr-mod').textContent = (mod>=0? '+'+mod : mod);
  });
  // update saves that depend on attributes
  updateSaveCharMods();
  updatePointsAvailable();
}

/* apply race & background modifiers to "adjust" fields */
function applyRaceAndBackground(){
  const race = $('raceSelect').value || 'Humano';
  const racial = RACES[race] && RACES[race].modifiers ? RACES[race].modifiers : {};
  document.querySelectorAll('.attr-adjust').forEach(inp=>{
    const attr = inp.getAttribute('data-attr');
    const val = racial && typeof racial[attr] === 'number' ? racial[attr] : 0;
    inp.value = val;
  });
  updateAttributeMods();
}

/* points available: simple calc: initial - sum(base) */
function updatePointsAvailable(){
  const totalBase = Array.from(document.querySelectorAll('.attr-base')).reduce((s,i)=> s + (parseInt(i.value)||0),0);
  const avail = Math.max(0, state.pointsAvailable - (totalBase - Math.floor(state.pointsAvailable/6)*6));
  $('points-available').textContent = avail;
}

/* size mapping by height (cm) */
function computeSizeByHeight(){
  const height = parseInt($('characterHeight').value) || 0;
  let size = "Mediano";
  if (height < 30) size = "Diminuto";
  else if (height < 60) size = "Menudo";
  else if (height < 120) size = "Pequeño";
  else if (height < 240) size = "Mediano";
  else if (height < 480) size = "Grande";
  else if (height < 960) size = "Enorme";
  else if (height < 2000) size = "Gargantuesco";
  else size = "Colosal";
  $('characterSize').value = size;
  state.size = size;
}

/* =========================
   Saves calculation
   Structure: for each save: total = base_clases + base_prestige + mod_char + mod_varios
   Where mod_char = ability modifier (CON for Fort, DES for Ref, SAB for Will)
==========================*/
function updateSaveCharMods(){
  const getAttrMod = (attr) => {
    const base = document.querySelector(`.attr-base[data-attr="${attr}"]`);
    const adj  = document.querySelector(`.attr-adjust[data-attr="${attr}"]`);
    const b = parseInt(base.value)||0, a = parseInt(adj.value)||0;
    return abilityModifier(b + a);
  };
  $('save-fort-char').value = getAttrMod('CON');
  $('save-ref-char').value  = getAttrMod('DES');
  $('save-will-char').value = getAttrMod('SAB');
  calcSaves();
}

function calcSaves(){
  ['fort','ref','will'].forEach(s=>{
    const base1 = parseInt($(`save-${s}-base`).value)||0;
    const base2 = parseInt($(`save-${s}-prestige`).value)||0;
    const mchar = parseInt($(`save-${s}-char`).value)||0;
    const mvar  = parseInt($(`save-${s}-mod`).value)||0;
    const total = base1 + base2 + mchar + mvar;
    $(`save-${s}-total`).textContent = total;
  });
}

/* =========================
   XP Calculation
   - computeNextLevelXP: sum of thresholds[targetLevel] for each class & prestige selected with level>0
   - show each class/prestige slot threshold as thresholds[targetLevel] * 1 (classes and prestiges treated same)
==========================*/
function computeNextLevelXP(){
  // determine total current effective level (sum of class levels + prestige levels)
  let totalLevel = 0;
  const classes = [];
  for (let i=1;i<=3;i++){
    const lvl = parseInt($(`class${i}-level`).value)||0;
    const name = $(`class${i}`).value || null;
    if (name && lvl>0) classes.push({name,lvl});
    totalLevel += lvl;
  }
  for (let i=1;i<=3;i++){
    const lvl = parseInt($(`prestige${i}-level`).value)||0;
    const name = $(`prestige${i}`).value || null;
    if (name && lvl>0) totalLevel += lvl;
  }
  const targetLevel = Math.max(2, totalLevel + 1); // at least level 2 threshold
  // fallback threshold if >20 -> scale linearly
  const getThreshold = (lvl) => {
    if (XP_THRESHOLDS[lvl]) return XP_THRESHOLDS[lvl];
    // simple extrapolation:
    const last = XP_THRESHOLDS[20];
    return Math.ceil(last * (lvl/20));
  };

  // compute per selected class/prestige the threshold for targetLevel
  for (let i=1;i<=3;i++){
    const name = $(`class${i}`).value;
    const cell = $(`xp-class-${i}`);
    if (name){
      cell.textContent = getThreshold(targetLevel).toLocaleString();
    } else cell.textContent = '—';
  }
  for (let i=1;i<=3;i++){
    const name = $(`prestige${i}`).value;
    const cell = $(`xp-pre-${i}`);
    if (name){
      cell.textContent = getThreshold(targetLevel).toLocaleString();
    } else cell.textContent = '—';
  }

  // sum them (only those chosen)
  let sum = 0;
  for (let i=1;i<=3;i++){
    if ($(`class${i}`).value) sum += getThreshold(targetLevel);
    if ($(`prestige${i}`).value) sum += getThreshold(targetLevel);
  }
  return sum;
}

/* XP add logic (mínimo implementado) */
function addXPSession(){
  const sess = parseInt($('xp-session').value)||0;
  if (sess<=0){ $('xp-log').textContent = "Introduce XP positiva"; return; }
  state.xpTotal += sess;
  const needed = computeNextLevelXP();
  if (state.xpTotal >= needed && needed>0){
    const overflow = state.xpTotal - needed;
    $('xp-log').innerHTML = `¡Subes 1 nivel! Requerías ${needed.toLocaleString()}. Tenías ${state.xpTotal.toLocaleString()}. ${overflow.toLocaleString()} XP se pierden.`;
    // increm. primer clase con nivel
    for (let i=1;i<=3;i++){
      const sel = $(`class${i}`);
      if (sel && sel.value){
        $(`class${i}-level`).value = Math.max(1, parseInt($(`class${i}-level`).value||0)+1);
        break;
      }
    }
    state.xpTotal = 0;
  } else {
    $('xp-log').textContent = `XP añadida: ${sess}. Total: ${state.xpTotal}. Necesitas ${needed} para subir.`;
  }
  $('xp-total').textContent = state.xpTotal.toLocaleString();
  $('xp-next').textContent = computeNextLevelXP().toLocaleString();
  $('xp-session').value = 0;
  refreshSidebarSummary();
}

/* export JSON */
function exportJSON(){
  const data = {
    name: $('characterName').value || state.name,
    player: state.player,
    race: $('raceSelect').value,
    alignment: $('alignment').value,
    deity: $('deitySelect').value,
    height: parseInt($('characterHeight').value)||0,
    size: $('characterSize').value,
    classes: [1,2,3].map(i=> ({ name: $(`class${i}`).value||null, level: parseInt($(`class${i}-level`).value)||0 })),
    prestiges: [1,2,3].map(i=> ({ name: $(`prestige${i}`).value||null, level: parseInt($(`prestige${i}-level`).value)||0 })),
    background: $(`backgroundSelect`).value,
    attributes: {}
  };
  document.querySelectorAll('.attr-row').forEach(row=>{
    const attr = row.querySelector('.attr-base').getAttribute('data-attr');
    const base = parseInt(row.querySelector('.attr-base').value)||0;
    const adjust = parseInt(row.querySelector('.attr-adjust').value)||0;
    const total = base + adjust;
    data.attributes[attr] = { base, adjust, total, modifier: abilityModifier(total) };
  });
  data.saves = {
    fort: parseInt($('save-fort-total').textContent)||0,
    ref: parseInt($('save-ref-total').textContent)||0,
    will: parseInt($('save-will-total').textContent)||0,
  };
  data.xp = { total: state.xpTotal, next: computeNextLevelXP() };
  const blob = new Blob([JSON.stringify(data,null,2)],{type:'application/json'});
  const url = URL.createObjectURL(blob);
  const a = document.createElement('a'); a.href=url; a.download=(data.name||'character')+'.json'; document.body.appendChild(a); a.click(); a.remove(); URL.revokeObjectURL(url);
}

/* sidebar summary refresh */
function refreshSidebarSummary(){
  const classes = [];
  for (let i=1;i<=3;i++){
    const name = $(`class${i}`).value; const lvl = parseInt($(`class${i}-level`).value)||0;
    if (name && lvl>0) classes.push(`${name} ${lvl}`);
  }
  $('charClasses').textContent = classes.length? classes.join(' / ') : '—';
  const prest = [];
  for (let i=1;i<=3;i++){
    const name = $(`prestige${i}`).value; const lvl = parseInt($(`prestige${i}-level`).value)||0;
    if (name && lvl>0) prest.push(`${name} ${lvl}`);
  }
  $('charPrestige').textContent = prest.length? prest.join(' / ') : '—';
  $('charName').textContent = $('characterName').value || state.name;
  $('playerNameDisplay').textContent = state.player || '—';
}

/* =========================
   Event listeners & init
==========================*/
function attachListeners(){
  // tabs
  document.querySelectorAll('.tab-btn').forEach(btn=>{
    btn.addEventListener('click', ()=>{
      document.querySelectorAll('.tab-btn').forEach(b=>b.classList.remove('active'));
      btn.classList.add('active');
      const t = btn.getAttribute('data-tab');
      document.querySelectorAll('.tab-content').forEach(tc=>tc.classList.remove('active'));
      $(t).classList.add('active');
    });
  });

  // avatar
  $('avatarInput').addEventListener('change', (e)=>{
    const f = e.target.files[0]; if (!f) return;
    const reader = new FileReader(); reader.onload = (ev)=>{ $('avatarImg').src = ev.target.result; $('avatarImg').style.display='block'; $('avatarLabel').style.display='none'; };
    reader.readAsDataURL(f);
  });

  // basic fields reactions
  $('characterHeight').addEventListener('input', ()=>{ computeSizeByHeight(); });
  $('raceSelect').addEventListener('change', ()=>{ applyRaceAndBackground(); refreshSidebarSummary(); });

  $('characterName').addEventListener('input', ()=> refreshSidebarSummary());
  $('playerName').addEventListener('input', (e)=> state.player = e.target.value );
  $('applyPlayer').addEventListener('click', ()=> { state.player = $('playerName').value; refreshSidebarSummary(); });

  // classes / prestige listeners
  for (let i=1;i<=3;i++){
    $(`class${i}`).addEventListener('change', ()=>{ refreshSidebarSummary(); $('xp-next').textContent = computeNextLevelXP().toLocaleString(); });
    $(`class${i}-level`).addEventListener('input', ()=>{ refreshSidebarSummary(); $('xp-next').textContent = computeNextLevelXP().toLocaleString(); });
    $(`prestige${i}`).addEventListener('change', ()=>{ refreshSidebarSummary(); $('xp-next').textContent = computeNextLevelXP().toLocaleString(); renderPrestigeDetail(); });
    $(`prestige${i}-level`).addEventListener('input', ()=>{ refreshSidebarSummary(); $('xp-next').textContent = computeNextLevelXP().toLocaleString(); renderPrestigeDetail(); });
  }

  // attribute inputs
  document.addEventListener('input', (e)=>{
    if (e.target.matches('.attr-base')) updateAttributeMods();
  });

  // saves inputs
  ['fort','ref','will'].forEach(s=>{
    $(`save-${s}-base`).addEventListener('input', calcSaves);
    $(`save-${s}-prestige`).addEventListener('input', calcSaves);
    $(`save-${s}-mod`).addEventListener('input', calcSaves);
  });

  // xp
  $('add-xp').addEventListener('click', addXPSession);
  $('exportBtn').addEventListener('click', exportJSON);
  $('saveBtn').addEventListener('click', ()=> alert('Guardar: pendiente almacenamiento (localStorage / servidor).'));
}

/* render prestige details (simple list) */
function renderPrestigeDetail(){
  const cont = $('prestige-detail'); cont.innerHTML = '';
  for (let i=1;i<=3;i++){
    const name = $(`prestige${i}`).value; const lvl = parseInt($(`prestige${i}-level`).value)||0;
    if (!name) continue;
    const el = document.createElement('div'); el.className='card'; el.style.marginBottom='8px';
    const saves = (PRESTIGE_LIST.find(p=>p.name===name)?.saves) || null;
    el.innerHTML = `<strong style="color:var(--gold)">${name} — Nivel ${lvl}</strong><div class="small" style="margin-top:6px">${saves? ('Patrón salvaciones: Fort '+saves.fort+' Ref '+saves.ref+' Vol '+saves.will) : 'Salvaciones: pendiente'}</div>`;
    cont.appendChild(el);
  }
}

/* initial render */
function init(){
  populateSelects();
  buildAttributesUI();
  attachListeners();
  computeSizeByHeight();
  applyRaceAndBackground();
  updateAttributeMods();
  calcSaves();
  $('xp-next').textContent = computeNextLevelXP().toLocaleString();
  $('xp-total').textContent = state.xpTotal.toLocaleString();
  refreshSidebarSummary();
  renderPrestigeDetail();
}

/* run init after DOM ready */
document.addEventListener('DOMContentLoaded', init);
</script>
</body>
</html>
