<%*
// Insert the exported light/dark SVG pair for an Excalidraw drawing.
const file = await tp.system.suggester(
  (f) => f.path,
  app.vault.getFiles()
    .filter((f) => f.path.endsWith(".excalidraw.md"))
    .sort((a, b) => a.path.localeCompare(b.path)),
  true,
  "Pick an Excalidraw drawing"
);
if (!file) return;

const folder = file.parent?.path && file.parent.path !== "/" ? file.parent.path : "";
const base = file.basename;
const path = (name) => folder ? `${folder}/${name}` : name;
const dark = path(`${base}.dark.svg`);
const light = path(`${base}.light.svg`);
const missing = [dark, light].filter((p) => !app.vault.getAbstractFileByPath(p));

if (missing.length) {
  new Notice(`Missing Excalidraw SVG export: ${missing.join(", ")}`);
}

tR += `![[${dark}]]\n`;
tR += `![[${light}]]\n`;
tR += `%% [[${file.path}|Edit in Excalidraw]] %%\n`;
%>
