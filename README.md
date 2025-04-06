<!DOCTYPE html>
<html lang="de">
<head>
  <meta charset="UTF-8" />
  <title>Analyse-Tool für TV-Beiträge (Finale XML mit KI-Anweisung)</title>
  <style>
    body { font-family: Arial, sans-serif; max-width: 960px; margin: auto; padding: 20px; }
    textarea { width: 100%; height: 160px; font-family: monospace; margin-top: 5px; }
    select, input[type="text"] { width: 100%; margin-top: 5px; padding: 6px; }
    .analyse-block { margin-bottom: 30px; border-top: 1px solid #ccc; padding-top: 20px; }
    .button-group { margin-top: 10px; display: flex; gap: 10px; flex-wrap: wrap; }
    label span { font-weight: normal; font-size: 14px; margin-left: 10px; color: #555; }
  </style>
</head>
<body>
  <h1>Analyse-Tool für Transkript & Anmoderation</h1>

  <label for="beitragstitel">Beitragstitel:</label>
  <input type="text" id="beitragstitel" placeholder="z. B. Sanierung des Großherzoglichen Palais" />

  <label for="ressort">Ressort:</label>
  <select id="ressort">
    <option value="">– Bitte wählen –</option>
    <option>Politik</option>
    <option>Wirtschaft</option>
    <option>Kultur</option>
    <option>Sport</option>
    <option>Wissen</option>
    <option>Gesellschaft</option>
    <option>Panorama</option>
    <option>Digitales</option>
    <option>Verbraucher</option>
    <option>Gesundheit</option>
    <option>Bildung</option>
    <option>Umwelt</option>
    <option>Verkehr</option>
    <option>Justiz</option>
    <option>Ausland</option>
  </select>

  <label for="tags">Tags (kommagetrennt):</label>
  <input type="text" id="tags" placeholder="z. B. Sanierung, Denkmal, Neustrelitz, Hotel" />

  <!-- TRANSKRIPT -->
  <div class="analyse-block">
    <h2>1. Transkript</h2>
    <label for="transkript">Text <span id="charCountTranskript">0 Zeichen</span></label>
    <textarea id="transkript" placeholder="Geben Sie hier das Transkript ein..."></textarea>

    <label for="weightTranskript">Gewichtung:</label>
    <select id="weightTranskript">
      <option value="0">0 %</option>
      <option value="25">25 %</option>
      <option value="50" selected>50 %</option>
      <option value="75">75 %</option>
      <option value="100">100 %</option>
    </select>
  </div>

  <!-- ANMODERATION -->
  <div class="analyse-block">
    <h2>2. Anmoderation</h2>
    <label for="anmoderation">Text <span id="charCountAnmoderation">0 Zeichen</span></label>
    <textarea id="anmoderation" placeholder="Geben Sie hier die Anmoderation ein..."></textarea>

    <label for="weightAnmoderation">Gewichtung:</label>
    <select id="weightAnmoderation">
      <option value="0">0 %</option>
      <option value="25">25 %</option>
      <option value="50" selected>50 %</option>
      <option value="75">75 %</option>
      <option value="100">100 %</option>
    </select>
  </div>

  <!-- FINALE XML -->
  <h2>3. Finale XML-Ausgabe</h2>
  <textarea id="finalXml" placeholder="Hier erscheint das finale XML..."></textarea>
  <div class="button-group">
    <button onclick="copyToClipboard()">In Zwischenablage kopieren</button>
    <button onclick="downloadXML()">XML herunterladen</button>
    <button onclick="generateFinalXML()">Finales XML erstellen</button>
  </div>

  <script>
    const transkriptInput = document.getElementById("transkript");
    const anmoderationInput = document.getElementById("anmoderation");
    const finalXmlOutput = document.getElementById("finalXml");

    transkriptInput.addEventListener("input", () => updateCharCount("transkript"));
    anmoderationInput.addEventListener("input", () => updateCharCount("anmoderation"));

    function updateCharCount(type) {
      const input = document.getElementById(type);
      const count = input.value.length;
      document.getElementById("charCount" + capitalize(type)).textContent = `${count} Zeichen`;
    }

    function capitalize(str) {
      return str.charAt(0).toUpperCase() + str.slice(1);
    }

    function escapeXml(unsafe) {
      return unsafe
        .replace(/&/g, "&amp;")
        .replace(/</g, "&lt;")
        .replace(/>/g, "&gt;")
        .replace(/"/g, "&quot;")
        .replace(/'/g, "&apos;");
    }

    function generateTimestamp() {
      const now = new Date();
      return now.toLocaleDateString("de-DE") + " " + now.toLocaleTimeString("de-DE");
    }

    function generateFinalXML() {
      const titel = escapeXml(document.getElementById("beitragstitel").value.trim());
      const ressort = escapeXml(document.getElementById("ressort").value);
      const tags = escapeXml(document.getElementById("tags").value.trim());

      const transkript = escapeXml(transkriptInput.value.trim());
      const anmoderation = escapeXml(anmoderationInput.value.trim());
      const weightT = document.getElementById("weightTranskript").value;
      const weightA = document.getElementById("weightAnmoderation").value;

      const timestamp = generateTimestamp();

      const xml = `<?xml version="1.0" encoding="UTF-8"?>
<online-ausgabe version="1.0">
  <quelle>
    <id>beitrag-${timestamp}</id>
    <art>TV-Beitrag</art>
    <titel>${titel}</titel>
    <themenvorgabe>
      <ressort>${ressort}</ressort>
      <tags>${tags}</tags>
    </themenvorgabe>
    <analysearten>
      <transkript gewichtung="${weightT}">
        <originaltext>${transkript}</originaltext>
        <analyse>
          <schritt nummer="2">Zentrale Fakten</schritt>
          <schritt nummer="3">Zentrales Thema</schritt>
          <schritt nummer="4">Informative Absicht</schritt>
          <schritt nummer="5">Beteiligte Personen oder Institutionen</schritt>
        </analyse>
      </transkript>
      <anmoderation gewichtung="${weightA}">
        <originaltext>${anmoderation}</originaltext>
        <analyse>
          <schritt nummer="2">Sprachlich-inhaltliche Analyse</schritt>
          <schritt nummer="3">Zentrale Fakten</schritt>
          <schritt nummer="4">Zentrales Thema</schritt>
          <schritt nummer="5">Informative Absicht</schritt>
          <schritt nummer="6">Prognose: Worum könnte es im Beitrag gehen?</schritt>
        </analyse>
      </anmoderation>
    </analysearten>
  </quelle>

  <editorial-metadata>
    <ueberschrift länge-max="60"></ueberschrift>
    <kurzbeschreibung länge-max="150"></kurzbeschreibung>
    <seo>
      <keywords max="10">
        <keyword></keyword>
      </keywords>
      <themen max="5">
        <thema></thema>
      </themen>
    </seo>
  </editorial-metadata>
</online-ausgabe>`;

      finalXmlOutput.value = xml;
    }

    function copyToClipboard() {
      finalXmlOutput.select();
      document.execCommand("copy");
    }

    function downloadXML() {
      const blob = new Blob([finalXmlOutput.value], { type: "application/xml" });
      const url = URL.createObjectURL(blob);
      const a = document.createElement("a");
      a.href = url;
      a.download = "finale-online-ausgabe.xml";
      a.click();
      URL.revokeObjectURL(url);
    }
  </script>
</body>
</html>
